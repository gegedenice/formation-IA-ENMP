# Fine-tuning de modèles

{% hint style="info" %}
Par construction, le fine-tuning n'est possible que sur les modèles open-weights.
{% endhint %}

{% hint style="info" %}
La plupart du temps le fine-tuning ne relève pas d'une étape de ré-entraînement unique mais d'un pipeline de phases successives de ré-entraînement où s'appliquent successivement plusieurs algorithmes (en général SFT + RLHF)
{% endhint %}

Le fine-tuning ou post-training consiste à réentraîner un modèle déjà pré-entraîné (par ex. un LLM ou un modèle d’embedding) sur un jeu de données spécialisé, dans le but d’adapter son comportement à :

* un domaine spécifique (ex. bibliothéconomie, droit, médecine…) ;
* une tâche précise (ex. classification, structuration de données, génération de notice Marc...) ;
* ou un style/tonalité particulier (ex. ton pédagogique, administratif, ou expert).

{% hint style="warning" %}
Quand choisir le fine-tuning de modèle ?

* Quand les autres solutions plus légères (qui ne modifient pas l'architecture du modèle : few-shot, RAG...) ne suffisent pas
* Quand on veut spécialiser un modèle sur une tâche particulière (pour augmenter les connaissances d'un modèle, choisir le RAG plutôt)
{% endhint %}

Il s’agit d’un ré-entraînement des poids internes du modèle (les paramètres appris pendant son pré-entraînement général) **qui sont donc modifiés durant le fine-tuning afin d'apprendre au modèle une compétence**,  par opposition au In-Context Learning (le RAG notamment) qui ajoute de la connaissance par injection de prompt.

Le post-training est donc un processus plus complexe et plus profond que le In-Context Learning car il ajuste structurellement la mémoire et la représentation interne des relations linguistiques du modèle

| **Caractéristique**                 | **Fine-tuning**                                                          | **In-Context Learning / RAG**                                         |
| ----------------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **Portée (ce qui est modifié)**     | Les poids du modèle (mémoire longue, stable)                             | Le contexte d’entrée (mémoire temporaire)                             |
| **Effet**                           | Le modèle _apprend réellement_ de nouvelles régularités ou comportements | Le modèle _lit et exploite_ des connaissances fournies à la volée     |
| **Durée de vie de la connaissance** | Permanente (apprise dans les poids)                                      | Éphémère (valide tant que présente dans le prompt ou le contexte RAG) |
| **Ressources nécessaires**          | GPU, pipeline d’entraînement, dataset structuré                          | Simple inférence avec index (embeddings + recherche)                  |
| **Cas d’usage typiques**            | Apprendre une tâche spécifique, un style, une structure de sortie        | Injecter des faits, documents ou bases de connaissances actualisées   |

## Principales méthodes de post-training

### Apprentissage supervisé : Supervised Fine-Tuning (SFT)

Le SFT est la méthode la plus simple et la plus couramment utilisée pour adapter un modèle de langage à un ensemble de données cible

#### Objectif

Apprendre à un modèle à imiter un comportement spécifique via des exemples sous forme de paires de séquences entrée → sortie.

Comme en Machine Learning classique, l'apprentissage est dit supervisé car les données sont labellisées : chaque observation en entrée est associée à une variable de sortie, et l’objectif de l’algorithme d'apprentissage consiste à associer observations et labels à partir des données.

#### Méthode

* On fournit un dataset de paires **input → output** (instructions + réponses attendues).

```
# Conversational language modeling
{"messages": [{"role": "user", "content": "What color is the sky?"},
              {"role": "assistant", "content": "It is blue."}]}

# Standard prompt-completion
{"prompt": "The sky is",
 "completion": " blue."}

# Conversational prompt-completion
{"prompt": [{"role": "user", "content": "What color is the sky?"}],
 "completion": [{"role": "assistant", "content": "It is blue."}]}
```

* On optimise le modèle via une loss classique (cross-entropy).

```
from trl import SFTTrainer
from datasets import load_dataset

trainer = SFTTrainer(
    model="Qwen/Qwen3-0.6B",
    train_dataset=load_dataset("trl-lib/Capybara", split="train"),
)
trainer.train()
```

* On monitore le process

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-11-16 132945.png" alt=""><figcaption></figcaption></figure>

#### Avantages

* Simple, stable, efficace même sur petits modèles.
* Contrôle précis du style, du format, de la conformité.

#### Limites

* Dépend fortement de la qualité du dataset.
* Risque d' "oubli catastrophique".

#### Exemple d'application : Instruction Tuning après le pre-training

Après la phase de pre-training, les modèles de base sont ré-entraînés pour apprendre à suivre des instructions multi-tâches (soit générales soit spécifiques à un domaine) de façon homogène.

_Voir la section sur l'_[_anatomie des modèles génératifs_](../module-2-ia-generative/modeles-generatifs-decoder-only/anatomie.md)

### Apprentissage par renforcement : Reinforcement Learning (RL) et Reinforcement Learning from Human Feedback RLHF&#x20;

#### Objectif

Apprendre au modèle à optimiser ses outputs sans lui fournir de données d'exemples préalables mais par un système de récompense qui oriente le modèle selon ses succès et ses échecs (sorte d'apprentissage par essais-erreurs) -> Les observations reçues par l’algorithme sont associées à des retours évaluatifs fournis par l’environnement et non pas des labels.

Dans le cadre des LLM qui génèrent du texte, il est difficile de faire du RL pur car il n'existe pas de méthode de récompense numérique qui permette de décider si une phrase est bonne ou mauvaise -> on fait donc du RL où la récompense provient de human feedbacks

#### Méthode

* S'appuie dur un dataset de préférences contenant réponses possibles classées par ordre de préférences selon des annotations humaines
* S'appuie sur un modèle de récompense "reward model" entraîné en SFT sur le dataset de préférences et qui apprend à prédire une récompense
* Variantes RLAI (RL from AI) : les préférences sont annotées par un LLM plus gros

#### Algorithmes

* Proximal Policy Optimization (PPO) :&#x20;
  * le modèle produit plusieurs réponses pour chaque prompt
  * le modèle de récompense évalue chaque réponse
  * l'algo de PPO met à jour les poids du modèle pour favoriser les réponses à haut score.
  * Ex de training dataset
    * RLHF : [https://huggingface.co/datasets/Anthropic/hh-rlhf](https://huggingface.co/datasets/Anthropic/hh-rlhf)
    * RLAI : [https://huggingface.co/datasets/edpowers/constitutional\_ai\_data](https://huggingface.co/datasets/edpowers/constitutional_ai_data)
* Variante Group Relative Policy Optimization GRPO : RL sans HF (sans les préférences)&#x20;
  * S'appuie sur une (ou plusieurs) fonction de récompense relative scalaire +1 / –1 (reward function) qui associe un score aux réponses du modèle
  * Le modèle produit un nombre donné de réponses possibles notées par la fonction de récompense et on optimise sur la réponse ayant la plus forte note
  * Associé à du test-time computing forçant le modèle à réf
* Variante Direct Preference Optimization (DPO) : préférences sans RL
  * S'appuie sur un dataset de préférences contenant des paires de prompt -> réponse gagnante choisie -> réponse perdante rejetée, ou des paires de prompts -> plusieurs réponses classées
  * On omet l’étape "reward model"  et en optimisant directement la probabilité de la bone réponse  en SFT.&#x20;
  * Ex de training dataset : [https://huggingface.co/datasets/openbmb/UltraFeedback](https://huggingface.co/datasets/openbmb/UltraFeedback)

#### Exemple en situation

On veux entraîner un modèle à produire des notices UNIMARC conformes à partir de raw data

* Avec PPO (RLHF)
  * des catalogueurs notent des notices : bonne/mauvaise.
  * on entraîne un reward model qui apprend ces préférences.
  * on entraîne un SLM par PPO qui l'optimise selon le reward model.
* Avec DPO
  * On crée un dataset de paires : notice avec erreurs vs notice correcte
  * On entraîne par DPO pour forcer le modèle à préférer la seconde.
* Avec GRPO
  * Le modèle génère 5 versions d’une notice.
  * On définit des règles dans des fonctions de récompenses (xml conforme, champ 200 présent...) et plus la notice respecte les règles plus le score est meilleur.
  * On entraîne par GRPO qui optimise selon ce score relatif.

#### Limites

* Forte sensibilité au modèle ou la fonction de récompense
* Coût de l'annotation, qu'elle soit humaine ou artificielle.

### Apprentissage par distillation

#### Objectif

Apprentissage par transfert des capacités d’un grand modèle vers un petit modèle spécialisé : on cherche à faire reproduire par un petit modèle les performances d’un grand modèle de langage en organisant un transfert de connaissances depuis le modèle plus avancé (dit teaching model) vers le modèle plus léger (dit student model).

Cette méthode de distillation de connaissances est notamment très efficace pour transformer un modèle de base en un modèle de raisonnement.

#### Méthode

* Sur la technique en elle-même, s'appuie sur du SFT classique avec des exemples de paires input -> output
* Deux formes de distillation
  * Distillation standard : le modèle étudiant imite les sorties de l’enseignant.
  * Distillation avec reasoning traces (ReFT) : le modèle apprend aussi les étapes intermédiaires de raisonnement.

#### Avantages

* Permet d’obtenir des SLM spécialisés performants.
* Idéal pour cas d’usage locaux / offline.

#### Limites

* Dépend de la qualité des traces du modèle enseignant.
* L'étudiant ne dépasse généralement pas l’enseignant.

## Le nerf de la guerre : les données d'entraînement

{% hint style="info" %}
Exemples de datasets d'entraînement

* GSM8K : [https://huggingface.co/datasets/openai/gsm8k](https://huggingface.co/datasets/openai/gsm8k)
* Open-R1-Math : [https://huggingface.co/datasets/open-r1/OpenR1-Math-220k](https://huggingface.co/datasets/open-r1/OpenR1-Math-220k)
{% endhint %}

Quelle que soit la technique de post-training utilisée, le facteur le plus déterminant n’est pas l’algorithme utilisé, mais la qualité, la quantité et la diversité du dataset d’entraînement.\
Un modèle ne devient pas bon parce qu’il a été fine-tuné, mais parce qu’il a été exposé aux bons exemples adaptés à la tâche.

Or bien souvent les données réelles manquent, et on utilise de plus en plus la génération de **données synthétiques,** qui consistent à faire produire par un modèle plus grand des exemples d’entraînement,  comme alternatives aux données issues de vérités terrain.

**Exemples de méthodes d'augmentation artificielle de données**

* {réponse} -> question (backtarnslation) : on remonte de la réponse à la question
* Question -> {réponse} : on infère la réponse à la question
* Question -> {raisonnement} -> réponse : on produit l'étape intermédiaire entre la question et la réponse
* {question} -> {réponse}

Pour de multiples raisons (droits d'auteur, données scrappées de mauvaise qualité, "mur des données"...), les données synthétiques sont amenées à jouer un rôle central dans le post-training mais aussi le pre-training -> voir à ce sujet le [projet SYNTH](https://pleias.fr/blog/blogsynth-a-practical-pipeline-for-synthetic-data-in-the-e-commerce) de Pleias.
