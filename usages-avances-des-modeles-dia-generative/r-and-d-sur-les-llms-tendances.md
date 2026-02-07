# R\&D sur les LLMs : tendances

## Context engineering vs prompt engineering :&#x20;

* Problématique : gestion architecturale du contexte vs gestion textuelle du prompt
* Comment designer, structurer (Knowledge graph) et donner à voir au modèle un contexte complet dans sa fenêtre de contexte  (mémoire statique, mémoire dynamique (conversations passées), exemples en fex-shot, documents, outils...)&#x20;

## Nouvelles architectures

* Les modèles sans tokenizer (tokenizer-free)
* Les modèles de diffusion (pour le texte)
* Modèles "à mémoire" auto-réfélechissants
* "World models"

## Post-training : la revanche des SLM

Entraînés sur des tâches très ciblées, les SLM (100M-1B) peuvent surpasser les LLM généralistes :

* Rapidité de l'inférence
* déploiement on-premise → souveraineté et RGPD
* comportements stables, contrôlables
* moins d’hallucinations après un bon post-training

### Fine-tuning sur raisonnement

### Enjeu des données synthétiques

{% embed url="https://huggingface.co/blog/catherinearnett/in-defense-of-tokenizers" %}

