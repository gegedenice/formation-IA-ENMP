# Anatomie

{% hint style="success" %}
**Notebook :** [**https://colab.research.google.com/drive/13XDjtNpuPEzElzKpI-Ow9x3pgTw3GMHj?usp=drive\_link**](https://colab.research.google.com/drive/13XDjtNpuPEzElzKpI-Ow9x3pgTw3GMHj?usp=drive_link)
{% endhint %}

{% hint style="info" %}
* **Attention** : Bidirectionnelle - chaque token "voit" tous les autres tokens simultanément
* **Méthode** : Masquer 15% des tokens et prédire les tokens masqués
* **Output** : Représentation vectorielle fixe (embedding) de dimension constante
{% endhint %}

## Principe de base

Un modèle d'embedding encode chaque texte (mot, phrase, document) en un vecteur de dimension fixe (souvent 384, 512, 768 ou 1024 dimensions). La proximité dans l'espace vectoriel reflète la similarité sémantique : deux textes similaires auront des vecteurs proches.

## Pre-training : exemple de Bert

Afin de capturer des représentations sémantisées du langage naturel dans des vecteurs, Bert est pre-entraîné sur de grandes quantités de texte brut (en anglais) selon la technique du langage masqué

* masquage au niveau des tokens : pour chaque phrase, le modèle masque de manière aléatoire 15 % des mots dans l’entrée, puis exécute toute la phrase masquée à travers le modèle qui apprend à prédire les mots masqués  **à partir du contexte** **des deux côtés**. Il permet au modèle d’apprendre une représentation bidirectionnelle de la phrase.

&#x20;      _Exemple : "Le chat est \[MASK] sur le canapé."_ → BERT devine "assis"

* masquage au niveau de la séquence : le modèle concatène deux phrases masquées en tant qu’entrées lors du pré-entraînement. Parfois, ces masques correspondent à des phrases qui étaient côte à côte dans le texte original, parfois non. Le modèle doit ensuite prédire si les deux phrases se suivaient ou non.

```
[CLS] Sentence A [SEP] Sentence B [SEP] -> Prédiction : probabilité de 0,5 que Sentence B suive Sentence A 
```

## Résultats

Exemple : [https://huggingface.co/google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased)

Un modèle est un ensemble de fichiers comprenant

* **le vocabulaire** : tous les tokens (mots, sous-mots, caractères spéciaux) que le modèle peut traiter et générer
* **le tokenizer** : le mappage token <-> ID et les règles de segmentation du texte (BPE, SentencePiece)
* **les poids du modèle** : fichiers binaires contenant les milliards de paramètres entraînés (souvent répartis en plusieurs fichiers .bin ou .safetensors)
* **des fichiers de configuration** : architecture du modèle, hyperparamètres, métadonnées d'entraînement (config.json)

## Exécuter le modèle (localement)

* On télécharge les fichiers en local
* Deux libraries python : transformers et sentence-transformers
* Le modèle convertit du texte en embeddings sémantiques

## Variantes du modèle Bert

De mutiples variantes et sous-variantes ont été entraînées par la communauté

* DistilBERT (distilbert-base-uncased...) : version allégée et plus rapide
* allenai/scibert-scivocab-uncased : spécialisé pour la littérature scientifique
* microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract : spécialisé bio-médical
* CamemBERT : entraîné sur des corpus français
* BertTopic

## Remarques

* Les résultats des modèles sont par nature très dépendants aux données d'entraînement : même en faisant en sorte que les données d’entraînement utilisées soient le plus neutres possible, le modèle reproduit les biais "culturels" des données dans ses prédictions.
* Gestion du multi-linguisme : le modèle comprend les langues des corpus sur lesquels il a été entraîné : corpus multi-lingue -> modèle multi-lingue
