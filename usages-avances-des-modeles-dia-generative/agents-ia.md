---
description: De la génération ponctuelle à l’exécution autonome, proactive et orchestrée.
---

# Agents IA

{% hint style="info" %}
**Paradigme** : passer d'outils de génération ponctuellement réactifs  à des systèmes d'exécution proactifs et adaptables

**Outils** : raisonnement séquentiel, prise de décisions autonome, activation orchestrée de tools,
{% endhint %}

Le développement des agents AI est basé sur l'observation pragmatique que, pour la réalisation de tâches complexes (avec des prompts monolithiques complexes), un LLM produit un résultat plus qualitatif en décomposant le prompt en ensemble de sous-tâches s'exécutant au sein d'un workflow avec itérations et feedbacks.

Couplé à des tools fournissant des accès à des outils KB externes ou la capacité de générer du code, un agent peut transformer et réaliser un objectif complexe en raisonnement **séquentiel, itératif et auto-orchestré.**

## Agent IA : définition

{% hint style="info" %}
Un agent = LLM + outils + planification + exécution + feedback.
{% endhint %}

Un agent IA est un **système d'exécution autonome** reposant sur un modèle de langage capable de générer du texte en mobilisant des capacités d'action, de raisonnement et d'adaptation pour atteindre un objectif.

Un agent IA est capable de :

* Planifier des séquences d'actions pour atteindre un objectif défini,
* Interagir avec des outils et systèmes externes
  * API et outils métiers (catalogue, SIGB, IdRef, OpenAlex, entrepôts OAI-PMH...)
  * bases de données internes
  *   fragments de code custom ou auto-générés


* S'adapter dynamiquement aux résultats obtenus et contraintes rencontrées,
* Collaborer avec d'autres agents ou systèmes au sein d'un environnement distribué.

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2026-01-23 183300.png" alt=""><figcaption></figcaption></figure>

<p align="center"><sub><em>Featured Image — Credit</em></sub> <a href="http://anthropic.com/"><sub><em>Anthropic</em></sub></a></p>

## Système multi-agents

{% hint style="info" %}
Un système multi-agents = plusieurs agents spécialisés orches­trés pour collaborer.
{% endhint %}

Un agent unique pouvant être limité pour des tâches vraiment complexes requérant des expertises multiples, on peut mettre en place des systèmes multi-agents, où plusieurs agents spécialisés se coordonnent, échangent de l'information et coopèrent au fil des itérations en simulant une **équipe de travail numérique**.

Chaque agent du système peut avoir accès à des outils suites de tools spécifiques et actionner des LLM différents.

#### Exemple d'architecture (classique) multi-agents

* **Planner** : décompose la tâche en étapes et organise le workflow.
* **Researcher** : collecte les données, fait les requêtes, interroge les bases documentaires.
* **Executor** : utilise les outils, exécute le code ou effectue les actions.
* **Reviewer** : vérifie la qualité, valide, propose des corrections.
