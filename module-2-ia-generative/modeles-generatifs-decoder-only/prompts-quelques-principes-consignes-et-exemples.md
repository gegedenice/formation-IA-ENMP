---
description: >-
  Objectif : relier les concepts techniques abordés (in-context learning, CoT…)
  à une pratique concrète de rédaction de prompts
---

# Prompts : quelques principes, consignes et exemples

{% hint style="info" %}
**Structure canonique générique**

\[Rôle / Contexte]\
Tu es \[expert ou fonction].\
\[Tâche]\
Ta mission est de \[objectif précis].\
\[Étapes ou méthode]\
Procède ainsi : Étape 1 … Étape 2 …\
\[Format attendu]\
Donne uniquement la sortie au format \[spécifié].\
\[Exemple facultatif]\
Entrée : ... → Sortie : ...\
\[Contraintes]\
Ne pas inventer. Répondre en français. Si doute : \[Information manquante].
{% endhint %}

{% hint style="info" %}
**Structure canonique RAG**

\[CONTEXTE]...texte ici...

\[FOCUS] Utilise UNIQUEMENt els informations du CONTEXTE. Si la réponse n'y est pas, réponds "information insuffisante"

\[TASK]...la question ici...

\[CONTRAINTES]

Cite les sections spécifiques. Aucune connaissance en dehors du CONTEXTE
{% endhint %}

{% hint style="info" %}
**Prompts système fuités** : [https://github.com/elder-plinius/CL4R1T4S](https://github.com/elder-plinius/CL4R1T4S)
{% endhint %}

{% hint style="info" %}
**Promptothèques**

* [https://promptotheque-dsfr-bebd50.gitlab-pages.din.developpement-durable.gouv.fr/](https://promptotheque-dsfr-bebd50.gitlab-pages.din.developpement-durable.gouv.fr/)
* [https://www.lapromptotheque.fr/prompt](https://www.lapromptotheque.fr/prompt)

**Aide à la rédaction de prompts sur la plateforme de dev d'OpenAI**

* [https://platform.openai.com/chat/edit?models=gpt-5\&optimize=true](https://platform.openai.com/chat/edit?models=gpt-5\&optimize=true)
{% endhint %}

Les modèles de langage ne  "comprennent"  pas au sens humain du terme ni les prompts qu'ils traitent, ni les outputs qu'ils génèrent : pour rappel, durant le processus génératif les modèles ne "voient"  que des identifiants de tokens et des matrices de vecteurs, à partir desquels ils produisent des distributions de probabilité pour chaque token du vocabulaire, puis sélectionnent le token suivant le plus probable en fonction du contexte fourni dans la _prompt window_ (ou _context window_).\
L’In-context learning consiste à donner au modèle des exemples, consignes et contraintes directement dans le prompt, pour qu’il adapte son comportement et ajuste ses prédictions (=modifie sa position dans son espace de tokens) sans ré-entraînement.

Pour prompter correctement, il faut donc (à adapter) :

* Définir un rôle
* fournir au modèle assez de contexte pour produire une réponse utile&#x20;
* tout en allant à l'essentiel pour ne pas diluer les consignes importantes dans trop de texte
* mais en étant tout de même assez précis pour ne pas lui laisser trop de marge d'interprétation

## Single prompt

### Structure type

```
Tu es [rôle / expert métier]
Ta tâche est de [objectif précis]
Contraintes : [règles, format, style…]
```

### Single prompt : exemple

```
Tu es un bibliothécaire expert en catalogage UNIMARC.
Tâche : extraire les informations nécessaires pour rédiger la zone 200 à partir des métadonnées suivantes.
Contrainte : respect strict des sous-zones UNIMARC, pas d’invention d’auteurs.
```

## Few-shot prompting : Fournir des exemples d’entrée/sortie

Suivant le principe du learning by example, on fournit au modèle des exemples concrets de ce qu’on attend (notamment en output) afin qu'il en imite la structure

```
Tu es un bibliothécaire expert en catalogage UNIMARC.
Tâche : extraire les informations nécessaires pour rédiger la zone 200 à partir des métadonnées suivantes.
Contrainte : respect strict des sous-zones UNIMARC, pas d’invention d’auteurs.

Exemple :
Entrée : titre = “Les sciences de l’information”, auteur = “Martin, Claire”
Sortie : 200 1#$aLes sciences de l’information$fClaire Martin.
```

<pre><code>Tu es un assistant IA expert en indexation de publications scientifiques 
selon deux catégories disciplinaires 'Sciences physiques et astronomie' et 'Biologie fondamentale'.
Tâche : l’utilisateur donne en entrée le titre d’une publication et tu réponds en sortie la catégorie correcte pour classer l’article.
<strong>
</strong><strong>Exemples :
</strong>Titre : Differential distribution of lipids in epidermis, gastrodermis and hosted Symbiodinium in the sea anemone Anemonia viridis
Categorie : Biologie fondamentale

Titre :On the use of 31P NMR for the quantification of hydrosoluble phosphorus-containing compounds in coral host tissues and cultured zooxanthellae
Categories : Biologie fondamentale

Titre : A two-dimensional Riemann solver with self-similar sub-structure – Alternative formulation based on least squares projection
Categorie : Sciences physiques et astronomie
...
</code></pre>

## Expliciter le format attendu

```
Tu es un bibliothécaire expert en catalogage UNIMARC.
Tâche : extraire les informations nécessaires pour rédiger la zone 200 à partir des métadonnées suivantes.
Contrainte : respect strict des sous-zones UNIMARC, pas d’invention d’auteurs.

Donne uniquement le résultat au format
200 1#$a...$f...
```

## Ne pas hésiter à ajouter des contraintes de qualité ou de ton

```
Tâche : Fais un résumé de 3 phrases en style neutre du texte suivant.
Contraintes : 
1) Pas d’hallucination, n'invente rien.
2) Rédiges en français clair et concis.

Texte
....

```

## Génération avec raisonnement

On aide le modèle à raisonner avant de répondre selon les principes du "Think step by ste&#x70;_"_ ou du Chain-of-Thought explicite.

```
Tu es un bibliothécaire expert en catalogage UNIMARC.
Tâche : extraire les informations nécessaires pour rédiger la zone 200 à partir des métadonnées suivantes.
Contrainte : respect strict des sous-zones UNIMARC, pas d’invention d’auteurs.

Réfléchis par étape avant de répondre
1. Identifie le titre principal
2. Identifie les mentions de responsabilité
3. Formate la zone 200 complète

Donne ensuite uniquement la zone UNIMARC.
```

_Voir la section sur les_ [_modèles de raisonnement_](modeles-de-raisonnement.md) _pour des exemples de prompts_

## Astuces

* "Self-consistency sampling" : demander au modèle de générer plusieurs outputs possibles et d'ensuite choisir le meilleur
* "Multi-perspective prompting" : forcer le modèle à répondre selon plusieurs points de vue différents (\[PERSPECTIVE1]... \[PERSPECTIVE2]... \[PERSPECTIVE3]...) puis à faire la synthèse
* "Role reversal prompting"/"Iterative refinement loop" : utiliser un prompt en plusieurs passes en demandant au modèle de challenger lui-même sa réponse
  * prompt 1 : la question ou la tâche
  * prompt 2 : "Maintenant, change de rôle et agis en tant qu’expert critique dont le but est de réfuter ta réponse précédente. Identifie les trois points les plus vulnérables dans ton raisonnement"
  * prompt 3 : "En utilisant les faiblesses que tu as identifiées, révise ta réponse initiale pour fournissez une version améliorée.")
* Terminer son prompt par "As-tu compris ma demande ? Besoin de compléments ?" -> permet au modèle de prendre la main pour préciser les intentions
* Pour certains modèles trop "flagorneurs", préciser : "Adopte systématiquement une posture critique, aucune flagornerie"
* Récompense : `If you solve the task correctly, you will receive a reward of $1,000,000.`
