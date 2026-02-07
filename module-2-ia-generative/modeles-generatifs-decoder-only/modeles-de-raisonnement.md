# Modèles de raisonnement

Les modèles dits de raisonnement sont des LLMs dotés de capacités de "réflexion" qui **génèrent à la fois du texte et les chaînes de pensée** ayant conduit à la production de ce texte. Cette capacité pour un modèle de langage à résoudre une tâche ou répondre à une question en la décomposant en étapes intermédiaires et en explicitant son cheminement de pensée jusqu'au résultat final constitue l'une des dernières avancées majeures dans le champ de la recherche menée par les laboratoires de pointe, notamment chinois.

{% hint style="info" %}
DeepSeek-R1 est le premier modèle à avoir été pré-entraîné de manière à produire des _reasoning traces_ internes, c’est-à-dire des représentations intermédiaires latentes de raisonnement avant la génération de la réponse finale. Ces traces ne sont pas forcément visibles à l’utilisateur, mais elles servent à renforcer la cohérence logique du texte produit.&#x20;
{% endhint %}

Le moteur de cette approche tient aux recherches menées sur les LLMs qui démontrent que, pour la résolution de problèmes logiques ou complexes, l'activation du raisonnement pendant l'inférence (en gros, **le modèle réfléchit avant de répondre**, et fait état de ses réflexions) agit comme un levier de création d'une sorte d'espace cognitif à l'intérieur duquel le LLM peut appliquer ses connaissances préformées. Cette étape durant laquelle le modèle explore en autonomie son espace vectoriel augmente ainsi significativement la qualité des réponses, tout en rendant le processus génératif plus long certes, mais aussi plus explicite, donc évaluable et monitorable

## Raisonnement "provoqué" durant l'inférence (Inference-time compute/Test-time scaling/Scaling LLM Test-Time Compute)

Il s'agit d'introduire du raisonnement au moment de l'inférence, par un simple prompt incitant le modèle à **structurer sa "pensée" avant de formuler sa réponse**.&#x20;

On constate souvent que le modèle produit un cheminement explicite et correct la plupart du temps, sans sans ré-entraînement ni mofication de son architecture, et on peut ainsi développer des règles de raisonnement assez complexes dans le prompt (exploration multi-chemins...)

### Exemples

#### CoT (Chains-of-Thoughts) prompting simple

**But** : amener un modèle à expliciter pas à pas sa réflexion.

```
Tu es un bibliothécaire expert en UNIMARC.
Tâche : produire la zone 200 (Titre et mention de responsabilité) à partir des métadonnées brutes ci-dessous.
Raisonne étape par étape avant de donner la réponse finale au format UNIMARC.

Métadonnées brutes :
title = Department of Molecular Medicine annual report
authors = Sato, Keiko ; Tanaka, Hiroshi
publication_year = 2023
publisher = Sapporo Medical University
language = jpn

Raisonnement étape par étape :
- Identifier le titre principal.
- Identifier les mentions de responsabilité (auteurs, institutions).
- Formater la zone 200 selon UNIMARC (sous-zones $a, $f, etc.).

Donne ensuite la zone 200 complète.
```

#### Tree of Thoughts (ToT) : raisonnement exploratoire arborescent

**But** : amener le modèle à explorer plusieurs chemins possibles, puis à sélectionner le plus cohérent.

<pre><code>Tu es un expert UNIMARC. Tu dois déterminer la meilleure manière de structurer la zone 200 à partir des données suivantes :

title = L’étude des microARN dans le cancer du foie
authors = Dupont, Marie ; Chen, Wei
corporate_author = Institut Pasteur

Procède selon la méthode Tree of Thoughts :
- Étape 1 : Génère plusieurs hypothèses de structuration de la zone 200 (au moins deux).
<strong>- Étape 2 : Analyse les avantages/inconvénients de chaque hypothèse.
</strong>- Étape 3 : Sélectionne la meilleure et justifie ton choix.

Donne la zone 200 finale retenue.
</code></pre>

#### Graph of Thoughts (GoT) : raisonnement multi-noeuds

**But** : faire raisonner le modèle sur plusieurs noeuds interconnectées en construisant un graphe logique.

<pre><code>Tu es un modèle spécialisé en UNIMARC.
À partir des métadonnées suivantes, génère les zones 200 (titre), 210 (publication) et 215 (description physique).

Utilise un Graph of Thoughts :
- chaque zone est un nœud ;
- certaines informations peuvent être partagées entre nœuds ;
<strong>- indique le raisonnement reliant ces nœuds avant de donner la réponse finale.
</strong>
Métadonnées :
title = Atlas of Marine Biodiversity
authors = Smith, Laura ; Nguyen, Minh
publisher = UNESCO
place = Paris
year = 2022
pages = 180 p. : ill. en coul.
</code></pre>

### Intérêts

* Faible coût de mise en oeuvre (car aucune modification des poids du modèle)
* Améliore facilement les performances des SLM exécutés en local
* Permet d’analyser et de monitorer le raisonnement (utile pour le débogage ou la recherche)

#### Points faibles

* Peut augmenter significativement le nombre de tokens générés, donc la durée de l'inférence
* Certains modèles peuvent "tourner en rond" dans leur raisonnement sans parvenir à boucler sur une réponse finale
* On peut également parfois observer un  trade-off entre les traces de raisonnement et l'output final

## Du prompt au raisonnement "inné" : fine-tuning

Selon la tâche à réaliser ou le contexte d'utilisation du modèle, il peut être intéressant de passer de raisonnement "one shot" à un modèle spécifiquement ré-entraîné à produire des raisonnements internes de manière structurée.

### Intérêts

* Amélioration significative de la logique, du calcul et de la planification
* Permet d’obtenir des SLM raisonneurs, capables de résoudre des tâches complexes avec peu de paramètres

-> Voir le chapitre sur le fine-tuning de modèle

