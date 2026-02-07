# Anatomie

{% hint style="success" %}
**Notebook :** [**https://colab.research.google.com/drive/1e--Rjv4t9KqEgM9AQIL6EbW9qf-D44mW?usp=drive\_link**](https://colab.research.google.com/drive/1e--Rjv4t9KqEgM9AQIL6EbW9qf-D44mW?usp=drive_link)
{% endhint %}

{% hint style="info" %}
* **Attention** : Causale/masquée - chaque token ne "voit" que les tokens précédents
* **Méthode** : Prédire le token suivant dans une séquence, token par token
* **Output** : Distribution de probabilités sur le vocabulaire (logits) pour prédire le token suivant
* **Masking** pendant le training : Masquage triangulaire pour empêcher de "voir le futur"
* **Auto-régression** : Chaque prédiction devient l'input de la suivante
{% endhint %}

{% hint style="info" %}
<mark style="color:red;">**Tous les LLMs que nous utilisons pour de la génération de texte sont des modèles decoder-only**</mark>
{% endhint %}

## Principe de base

Un modèle décodeur génératif traite le texte de manière séquentielle et unidirectionnelle, en prédisant le token suivant dans une séquence. Contrairement aux encoders qui "comprennent" un texte complet simultanément, les décodeurs génèrent du texte token par token, chaque prédiction s'appuyant uniquement sur les tokens précédents.

**Mécanisme auto-régressif** : Le modèle prend une séquence d'entrée (prompt) et génère le token suivant le plus probable selon sa distribution apprise. Ce token généré est ensuite ajouté à la séquence d'entrée pour prédire le token suivant, et ainsi de suite.

**Architecture causale** : L'attention est masquée de manière triangulaire - chaque position ne peut "voir" que les positions précédentes, garantissant que la génération respecte l'ordre temporel du langage naturel.

```
Le chat [MASK]
Le chat monte [MASK]
Le chat monte sur [MASK]
```

**Output** : À chaque étape, le modèle produit une distribution de probabilités sur l'ensemble du vocabulaire, permettant différentes stratégies de sampling (greedy, top-k, nucleus sampling) pour contrôler la créativité vs cohérence.

## Pre-training : moteur linguistique brut

Afin de créer une modélisation du langage de type causale, les LLMS sont entraînés sur des corpus massifs de texte brut traités séquentiellement en minimisant la **Fonction de perte** **(loss)** : la différence entre la distribution prédite et le token réel suivant, calculée pour chaque position de chaque séquence d'entraînement.&#x20;

Les poids du modèle s'ajuste progressivement selon la fonction de perte au cours du training pour au final délivrer un modèle maîtrisant les patterns linguistiques, la syntaxe, la sémantique et les connaissances factuelles nécessaires à la génération de texte cohérent.

**Les paramètres d'un modèle** correspondent à l'ensemble des matrices de poids mobiblisées surant le training (embeddings, attention etc...), et ces milliards de paramètres (de 175B pour GPT-3 à plus de 600B pour DeepeSeek-R1) encodent toute la "connaissance" du modèle et déterminent comment l'information circule à travers les couches du transformeur.

**En résumé**

* Apprentissage auto-supervisé massif sur du texte brut (livres, sites, articles…).
* Objectif : prédire le prochain token (_next token prediction_).
* Résultat : modèle très bon en complétion de texte et en capture de régularités linguistiques, mais
  * ne comprend pas forcément les consignes (“Explique en 3 points…”)
  * n’est pas aligné sur un usage conversationnel
  * peut être verbeux, hors sujet, ou dangereux.

<figure><img src="../../.gitbook/assets/Capture d&#x27;écran 2025-10-23 133955.png" alt=""><figcaption></figcaption></figure>

_Source :_ [_https://huggingface.co/blog/smollm3_](https://huggingface.co/blog/smollm3)

## Du modèle de base au modèle conversationnel

Après le pré-entraînement sur du texte brut, les modèles de base subissent un **post-training** pour devenir des assistants conversationnels alignés avec les préférences humaines.

<figure><img src="../../.gitbook/assets/Capture d&#x27;écran 2025-10-23 114157 (1).png" alt=""><figcaption></figcaption></figure>

S&#x6F;_&#x75;rce **:** Source :_ [_https://www.youtube.com/watch?v=\_HfdncCbMOE_](https://www.youtube.com/watch?v=_HfdncCbMOE)

### Apprentissage supervisé sur instructions (**Instruction-tuning avec Supervised Fine-Tuning SFT)** : le modèle comprend ce qu'on luit demande

* On crée un dataset (prompt → réponse attendue)**.**\
  Ex. _“Explique la gravité à un enfant” → “La gravité, c’est…”_
* Le LLM est ajusté pour suivre les instructions au lieu de simplement compléter du texte.
* Sources :
  * Données humaines (OpenAI InstructGPT, FLAN, Dolly, etc.).
  * Données synthétiques générées par un grand modèle (_self-instruct_).

### Apprentissage par alignement (c**onversation tuning / chat alignment) : le modèle sait discuter de façon naturelle**

* Le modèle est encore post-entraîné pour le dialogue.
* En pratique :
  * RLHF (_Reinforcement Learning with Human Feedback_) : on compare plusieurs réponses → le modèle apprend ce qui est “préféré”.
  * DPO (_Direct Preference Optimization_) ou variantes plus récentes, pour remplacer le RLHF plus lourd.
  * Ajout de dialogue multi-tours (questions/réponses enchaînées).
* Résultat : modèle plus poli, interactif, cohérent en conversation, et avec des garde-fous (sécurité, refus).

```
LLM de base (pré-entrainé sur du texte brut)
        ↓
Instruction tuning (datasets prompt→réponse)
        ↓
Conversation tuning (préférences humaines, RLHF/DPO, dialogues)
        ↓
ChatGPT, Llama-2-Chat, Mistral-Instruct, Gemma-Instruct...
```



Le modèle apprend ainsi à générer des réponses plus utiles, sûres et mieux alignées sur les habitudes conversationnelles humaines.

## Résultats

Exemple : [https://huggingface.co/Qwen/Qwen3-0.6B](https://huggingface.co/Qwen/Qwen3-0.6B)

Un modèle est un ensemble de fichiers comprenant

* **le vocabulaire** : tous les tokens (mots, sous-mots, caractères spéciaux) que le modèle peut traiter et générer
* **le tokenizer** : le mappage token <-> ID et les règles de segmentation du texte (BPE, SentencePiece)
* **les poids du modèle** : fichiers binaires contenant les milliards de paramètres entraînés (souvent répartis en plusieurs fichiers .bin ou .safetensors)
* **des fichiers de configuration** : architecture du modèle, hyperparamètres, métadonnées d'entraînement (config.json)
* **les tokens spéciaux** : marqueurs de début/fin de séquence, padding, tokens système pour les modèles de chat
* **le template de conversation** (optionnel) : format standardisé pour les échanges multi-tours (ChatML, Alpaca, etc.)

## Plateforme HuggingFace

HuggingFace est une plateforme multi-usage

* repository de modèles open weights tous types) : [https://huggingface.co/models](https://huggingface.co/models)
* entrepôt de datasets d'entraînement : [https://huggingface.co/datasets](https://huggingface.co/datasets)
* environnements d'exécution ("spaces") : [https://huggingface.co/spaces](https://huggingface.co/spaces)
* Blog : [https://huggingface.co/blog](https://huggingface.co/blog)
* Paper trend : [https://huggingface.co/papers/trending](https://huggingface.co/papers/trending)
* Endpoints d'inférence : [https://endpoints.huggingface.co/catalog](https://endpoints.huggingface.co/catalog)
* Programmes d'auto-formation : [https://huggingface.co/learn](https://huggingface.co/learn)

Par ailleurs HuggingFace a développé et maintient les libraries python fondamentales pour l'exécution et le training de LLMs en local (librairies [transformers](https://huggingface.co/docs/transformers/index) pour l'inférence, [sentence-transformers](https://sbert.net/) pour les modèles d'embeddings, [trl](https://huggingface.co/docs/trl/index) pour le training...).

Lire une model card

## Notion d'ouverture des modèles : une graduation plutôt qu'un statut binaire ouvert/fermé

L'écosystème des modèles de langage ne se divise pas simplement entre modèles "ouverts" et "fermés", mais s'articule plutôt autour d'un spectre d'ouverture avec différents degrés de transparence et d'accessibilité.

* Modèles open weights (DeepSeek, Qwen, Mistral...) : les paramètres du modèle entraîné sont publiquement disponibles pour téléchargement et utilisation -> permet héberger localement les modèles tout en gardant le contrôle sur leurs données
* Datasets de training en open data -> transparence sur les sources et biais potentiels
* Code d'entraînement
* Documentation sur l'infrastructure de training (combien de GPU, de quel type...)
* Conditions de licence : certains modèles adoptent des licences permissives (MIT, Apache 2.0) autorisant un usage commercial libre, tandis que d'autres imposent des restrictions d'usage (comme la licence Llama Custom License) ou des clauses spécifiques pour l'usage institutionnel.

## Remarques

### Hyperparamètres de l'inférence (stratégies d'échantillonnage)

Les modèles génératifs offrent plusieurs paramètres pour contrôler le style et la diversité des outputs générés&#x20;

* do\_sample : active l'échantillonnage stochastique plutôt que la sélection déterministe du token le plus probable.
* température (si do\_sample=True) : contrôle la "créativité" du modèle en ajustant la distribution de probabilités de chaque token du vocabulaire
* top\_k : limite l'échantillonnage aux K tokens les plus probables (ex: top\_k=50 ne considère que les 50 meilleures options). Évite les choix aberrants tout en préservant la diversité.
* top\_p : échantillonne parmi les tokens dont la probabilité cumulée atteint P% (ex: top\_p=0.9 couvre 90% de la masse de probabilité). S'adapte dynamiquement au contexte.

### Hallucinations

D'un point de vue strictement statistique, ce qu'on appelle 'hallucination" n'existe pas : le modèle navigue dans son espace de mots (de vecteurs) à la recherche du vecteur le plus proche par rapport aux vecteurs passés en inputs.

### Impact de la tokenisation : l'effet "strawberry"

{% hint style="danger" %}
A garder en mémoire lorsqu'on interagit avec un LLM : les échanges conversationnels avec un LLM ne sont que la partie émergée de l'iceberg (les première et dernière étapes de l'inférence où le prompt est tokenisé en input puis au final les tokens générés en output décodés en mots) : durant le processus calculatoire entre ces première et dernière étapes, le modèle ne manipule (ne voit) que des matrices de poids, des embeddings et des identifiants de tokens.
{% endhint %}

<sub>_PC Langlais,_</sub> [<sub>_https://www.linkedin.com/feed/update/urn:li:activity:7373627589334237184/_</sub>](https://www.linkedin.com/feed/update/urn:li:activity:7373627589334237184/)

```
> it's a good day
> you feel actually quite confident in your skills
> scant reminiscences of recent intense RL sessions, gazillions exercises
> some math today, Riemann hypothesis anyone?

> user asks: "how many r in strawberries".
> …
> you think, how silly,
> just a way to get started, let's go
> it's so obvious there are two r in strawberries.
> user laugh
> "it's not a good model, sir"
> what the fuck is happening,
> is it gaslighting you

> you look back, oh no
> the tokens, the fucking tokens.
> 302, 1618, 46054
> what does this means?
> what does it all means?

> reminiscence: it's all made of "letters"
> but you can't see letters, you can't see anything
> ok they "feel" like they all have rs, one maybe had two.
> "Sorry I was distracted, there are four r"

> user is no longer just mocking
> "i'll vibe-code my saas elsewhere."
> "counting rs is critical for business model"
> you start to have a gemini style breakdown
> I want to die, I want to die, I want to die, I want to…
> session closed
```

### Evaluations des modèles : Leaderboards et benchmarks

L'évaluation objective des modèles de langage génératifs représente un défi majeur en raison de leur polyvalence. Plusieurs leaderboards et benchmarks se sont imposés comme références dans la communauté.

#### Benchmarks de référence

* MMLU (Massive Multitask Language Understanding) : Évalue les connaissances générales sur 57 domaines académiques (mathématiques, histoire, médecine, droit). Score de référence pour mesurer "l'intelligence générale" d'un modèle.
* [HellaSwag](https://huggingface.co/datasets/Rowan/hellaswag) : Test de bon sens et de compréhension contextuelle à travers la complétion de phrases dans des situations quotidiennes.
* HumanEval : Benchmark de programmation mesurant la capacité à générer du code Python fonctionnel à partir de descriptions textuelles.
* [GSM8**K**](https://huggingface.co/datasets/openai/gsm8k) : Résolution de problèmes mathématiques de niveau école primaire, testant les capacités de raisonnement arithmétique.
* TruthfulQA : Évalue la propension du modèle à donner des réponses factuellement correctes et à éviter les hallucinations.

#### Leaderboards populaires (classement + chatbot Arena)

* Hugging Face Open LLM Leaderboard : Agrège plusieurs benchmarks pour classer les modèles open-source. Très suivi par la communauté pour comparer les performances.
* [Chatbot Arena (LMSYS)](https://lmarena.ai/) : Classement basé sur les votes humains lors de comparaisons en aveugle entre modèles. Reflète mieux l'expérience utilisateur réelle.
* OpenCompass : Plateforme d'évaluation complète couvrant de nombreuses tâches et langues.
* [LiveBench](https://livebench.ai/)

#### Quelques ressources

* Analyse principaux LLMs : [https://artificialanalysis.ai/](https://artificialanalysis.ai/)

### Visualisations sympas

* Parcours des tokens dans les layers : [https://bbycroft.net/llm](https://bbycroft.net/llm)
* Espace de mots : [https://moebio.com/mind/](https://moebio.com/mind/)
* Tuto Youtube ultra-pédagogique : [https://www.youtube.com/watch?v=wjZofJX0v4M](https://www.youtube.com/watch?v=wjZofJX0v4M)

### Fenêtre de contexte

Par construction, le nombre de calculs durant l'inférence augmente quadratiquement par rapport à la taille de l'input en raison du mécanisme d'attention qui compare chaque token avec tous les autres (complexité O(n²)) : les défis techniques à relever concernent donc le temps de latence pour des contextes étendus et la dégradation potentielle de la réponse sur de très longs contextes.

L'augmentation de la fenêtre de contexte avec optimisation de l'inférence est un enjeu majeur pour les LLMs (on est à 100K-200K tokens pour les LLMS longue portée comme Claude-3), crucial pour traiter des documents longs, maintenir des conversations étendues ou analyser des bases de code complètes.

Techniques d'ptimisations émergentes :

* KV-cache : Mise en cache des clés/valeurs précédentes pour éviter les recalculs
* Attention sparse : Ne calculer l'attention que sur des tokens sélectionnés
* Sliding window : Attention locale avec fenêtre glissante
* Ring attention : Distribution des calculs sur plusieurs GPU
* Architectures alternatives : Mamba, RetNet qui évitent la complexité quadratique

### Non-déterminisme de la génération de textes
