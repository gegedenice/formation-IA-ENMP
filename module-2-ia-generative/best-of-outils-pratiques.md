---
description: >-
  Conseils, recommandations et astuces pour explorer sans code (no-code) ou avec
  peu de code
---

# Best of outils pratiques

L’objectif de cette page est de donner des pistes simples et accessibles pour tester des modèles, explorer leurs représentations, et faire tourner localemen**t** des LLM.

## Tester facilement des modèles (sans installer quoi que ce soit)

### Chatbots et playgrounds en ligne

* Playground HugingFace : [https://huggingface.co/playground](https://huggingface.co/playground) (ou accessible depuis une model card "Open Playground")
* HuggingChat : [https://huggingface.co/chat/](https://huggingface.co/chat/)
* Chatbot Arena : [https://lmarena.ai/?mode=side-by-side](https://lmarena.ai/?mode=side-by-side)
* Duck.ai : [https://duckduckgo.com/?q=DuckDuckGo+AI+Chat\&ia=chat\&duckai=1](https://duckduckgo.com/?q=DuckDuckGo+AI+Chat\&ia=chat\&duckai=1)
* Jan Web (petits modèles très performants) : [https://chat.jan.ai/](https://chat.jan.ai/)
* Pensez à tester les modèles chinois :&#x20;
  * Modèles Qwen : [https://chat.qwen.ai/](https://chat.qwen.ai/)
  * Modèels DeepSeek : [https://chat.deepseek.com/](https://chat.deepseek.com/)
  * Modèles Moonshotaï : [https://www.kimi.com/](https://www.kimi.com/)
* Pensez à ComparIA

### HuggingFace

La plateforme HugingFace, au-delà du simple repository de modèles et datasets, est une plateforme complète d'applications IA prêtes à l'emploi via des espaces d'exécution créés par la communauté autour des modèles et des widgets d'inférence rapide grâce à des providers multiples.

#### Inférence rapide depuis une model card

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-11-13 153119.png" alt=""><figcaption></figcaption></figure>

#### Spaces

Depuis l'onglet Spaces des model cards

_Ex :_ [_https://huggingface.co/spaces/baohuynhbk14/Qwen3-VL-Demo_](https://huggingface.co/spaces/baohuynhbk14/Qwen3-VL-Demo)

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-11-13 155106.png" alt=""><figcaption></figcaption></figure>

## Explorer/visualiser les embeddings

### Plateforme en ligne : [Nomic Atlas](https://atlas.nomic.ai/discover)

Ex : [https://atlas.nomic.ai/data/geraldine.geoffroy/enmp-openalex-dataset/map/8d7a8d8f-f8b2-4a3e-88f0-d75cb2e9b16b#jmmy](https://atlas.nomic.ai/data/geraldine.geoffroy/enmp-openalex-dataset/map/8d7a8d8f-f8b2-4a3e-88f0-d75cb2e9b16b#jmmy)

### Librairies Python open source

#### Embeddings-atlas

{% hint style="info" %}
A noter : export possible de la dataviz en format HTML
{% endhint %}

Doc : [https://apple.github.io/embedding-atlas/](https://apple.github.io/embedding-atlas/)

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-11-13 155525.png" alt=""><figcaption></figcaption></figure>

#### Latent Scope

{% hint style="info" %}
Axé démarche scientifique reproductible avec sauvegarde locales des données + paramètres à chaque étape
{% endhint %}

Doc : [https://enjalot.github.io/latent-scope/](https://enjalot.github.io/latent-scope/)

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-11-13 155639.png" alt=""><figcaption></figcaption></figure>

## Faire de l'inférence avec des modèles génératifs en local

### **Ollama (ligne de commande)**

* Developpé sur llama.cpp
* Active un serveur web local d'inférence.
* Pull et run de modèles quantifiés&#x20;
* Usage : `ollama run llama3` ou  `ollama pull glm-4.7:cloud`  &#x20;
* Mini-chatbot en ligne possible

#### **Jan (desktop)**

* Version “grand public” de Ollama, avec interface conviviale.
* Modèles locaux quantifiés, gestion des “personas”, historique des conversations.

#### **LM Studio (desktop)**

* Alternative graphique avancée, multiples paramétrages
* Modèles locaux quantifiés
* Inférence optimisée

#### **llama.cpp (ligne de commande, pour utilisateurs avancés)**

## **Faire de l'inférence par API**

Ollama, Grok, Albert API



## Applications web open source pour construire des prototypes de RAG et d’agents (sans coder)

### OpenWebUI

### RAGFlow&#x20;

* Très complet sur le paramétrage du RAG, Search et Chat embeddable dans page web
* Doc : [https://ragflow.io/docs/dev/](https://ragflow.io/docs/dev/)
* Démo en ligne : [https://demo.ragflow.io/](https://demo.ragflow.io/)

### Flowise

* Tool “low code”, workflows visuels avec drag & drop.

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-11-13 180549.png" alt=""><figcaption></figcaption></figure>

## Applications cloud agentique



* NotebookLM : [https://notebooklm.google.com/](https://notebooklm.google.com/)
  * Alternative open source avec modèles ouverts : Open-notebook [https://github.com/lfnovo/open-notebook](https://github.com/lfnovo/open-notebook)
* Manus : [https://manus.im/app](https://manus.im/app)
  * Alternative opne source : OpenManus [https://openmanus.github.io/](https://openmanus.github.io/)

## Les nouveaux agents de vibe coding

Claude Code, Gemini CLI, Codex, Open Code, Mistral Vice

Vibe Kanban
