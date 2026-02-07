# Inférence : solutions techniques

{% hint style="success" %}
**Notebook :** [**https://colab.research.google.com/drive/1U5c0PS9BatKRoyRhYSQ7QDE92AVSSaBS?usp=drive\_link**](https://colab.research.google.com/drive/1U5c0PS9BatKRoyRhYSQ7QDE92AVSSaBS?usp=drive_link)
{% endhint %}

## Chatbots en ligne

### Chatbots de fournisseurs

* Modèles fermés : ChatGPT, Claude, [Mistral](https://chat.mistral.ai/chat), [Gemini](https://gemini.google.com/)
* Modèles ouverts : [Alibaba](https://chat.qwen.ai/) (modèles Qwen), [DeepSeek](https://chat.deepseek.com/), [Moonshotai](https://www.kimi.com/) (modèles Kimi), [Ai2](https://playground.allenai.org/), [LiquidAI](https://playground.liquid.ai/), [Jan Web](https://chat.jan.ai/)

### Playgrounds (chatbot avec choix de plusieurs modèles)

* HuggingFace playground : [https://huggingface.co/playground](https://huggingface.co/playground)
* HuggingChat : [https://huggingface.co/chat/](https://huggingface.co/chat/)
* Chat Langchain : [https://chat.langchain.com/](https://chat.langchain.com/)
* [Compar'IA](https://beta.gouv.fr/startups/languia.html) : comparer (et évaluer) les réponses de plusieurs LLMs sur des tâches en français
* cf chatbot Arena dans [chapitre sur l'évaluation](anatomie.md)

## API distante

{% hint style="info" %}
* LLMs distants : hébergés par un provider
* LLMs ouverts ou fermés
* APIs payantes (sauf HuggingFace et free-tiers Groq)
* Permet d'intégrer les réponses de LLMs dans un environnement applicatif (y compris d'automatisation)
{% endhint %}

En général la plupart des fournisseurs de LLMs proposent, en plus des IHM type chatbot, des API Rest pour interagrir avec leurs LLMS avec une facturation à l'usage (pay-as-you-go sur le nombre de tokens générés)

### Standard d'API OpenAI-like

{% hint style="warning" %}
L'API permet de faire des requêtes sur des modèles génératifs et des modèles d'embeddings
{% endhint %}

OpenAI a créé un format d'interaction standardisé pour communiquer par API Rest avec les modèles de langage. Ce format est devenu si populaire que la quasi-totalité des fournisseurs l'ont adopté, créant un standard de facto dans l'industrie.

L'API est compatible avec le format de base (prompt simple) et le format conversationnel des modèles. Dans ce cas les échanges sont structurés comme une conversation avec des rôles définis :

* "system" : Instructions générales pour définir le comportement du modèle
* "user" : Messages de l'utilisateur humain
* "assistant" : Réponses du modèle IA

L'API permet également de gérer:

* les paramètres de contrôle : température, top\_p, top\_k, max\_tokens...
* le format de l'output (pour l'extractio de données structurées par exemple)
* l'envoi de médias (image, audio..)
* le recours à des fonctions (systèmes agentiques)

```
#Exemple de data passées en POST à https://api.openai.com/v1/chat/completions
{
  "model": "gpt-4",
  "messages": [
    {"role": "system", "content": "Tu es un super assistant"},
    {"role": "user", "content": "Explique-moi les LLMs"}
  ],
  "temperature": 0.7,
  "max_tokens": 500
}
```

### Fournisseurs propriétaires (API natives)

OpenAI (leader historique, tous modèles),  Anthropic, Google, Mistal AI, DeepSeek

### Plateformes d'aggrégation

* [Ollama Cloud](https://ollama.com/blog/cloud-models) (gratuit) : gros LLM hébergés sur le cloud public Ollama et accessibles par API
* [Groq](https://console.groq.com/docs/overview) : Inférence ultra-rapide sur puces spécialisées (LPU), free-tiers très intéressant
* [Hugging Face Inference API](https://huggingface.co/docs/inference-providers/index) (gratuit) : Accès simplifié aux modèles du Hub&#x20;
* [OpenRouter](https://openrouter.ai/models),  [Together AI](https://www.together.ai/), [Replicate](https://replicate.com/) (payant) : API unifiée pour l'accès aux principaux LLMs du marché par une plateforme Cloud.
* [API Perplexity](https://docs.perplexity.ai/getting-started/overview)  : API spécialisée recherche augmentée

### Limites à un usage courant

* Coût à l'usage
* **Pas de confidentialité des données : quand on utilise un chatbot en ligne ou une API, toutes les données entrées (query, documents) sont traitées sur le serveur distant du fournisseur**
* Selon le domaine métier ou la spécificité de la question, les données de pre-training du modèle peuvent porter des biais culturels (sur-représentation US...)

## Solutions auto-hebergées pour modèles open weights

### Intérêt

* Pouvoir exécuter un LLM en local sans fuite de données
* Faire de l'inférence sur son PC avec des Small Language Models (SLM) qui peuvent se montrer plus performants sur certaines tâches que des gros LLMs généralistes
* Mettre en place une API locale sur un LLM auto-hebergé

### Quantification (quantization)

Exécuter un LLM requiert beaucoup de ressources RAM et CPU/GPU (espace disponible pour gros modèles à plusieuer Go + mémoire vive nécessaire pour les calculs matriciels) -> La solution pour pouvoir exécuter des LLMs sur des infras limitées (y compris sans GPU) consiste à délivrer des versions "compressées" des modèles.

#### Principe de la quantification

* Réduction de précision : Les poids du modèle, initialement stockés en float32 (32 bits) ou float16 (16 bits), sont convertis en formats plus légers : int8 (8 bits), int4 (4 bits), voire int2.
* Compression drastique : Un modèle 7B passe de \~14 GB (float16) à \~4 GB (4-bit), soit une réduction de 75% avec une perte de qualité minimale.

Exemple :&#x20;

* Modèle natif non compressé : [https://huggingface.co/HuggingFaceTB/SmolLM3-3B](https://huggingface.co/HuggingFaceTB/SmolLM3-3B)
* Le même modèle disponible en plusieurs versions quantifiées : [https://huggingface.co/bartowski/HuggingFaceTB\_SmolLM3-3B-GGUF](https://huggingface.co/bartowski/HuggingFaceTB_SmolLM3-3B-GGUF)

#### Impacts pratiques

* Démocratisation : Modèles 7B-13B exécutables sur ordinateurs portables standards (8-16 GB RAM).
* Déploiement edge : IA générative sur appareils mobiles, embarqués, ou environnements contraints.
* Trade-offs : Légère dégradation de qualité compensée par l'accessibilité. Les modèles quantifiés 4-bit conservent généralement 95-98% des performances originales.

La quantification transforme des modèles "enterprise-only" en solutions accessibles au grand public et aux petites structures.

### Outils

Il existe un grand nombre d'outils et d'applications permettant de télécharger et exécuter en local des modèles quantifiés.

{% hint style="info" %}
A noter que la plupart de ces applications une fois installées activent également un serveur local OpenAI-like, rendant possible les requêtes d'inférence par API (sur une url du type http://localhost sur PC ou sur le nom de domaine du serveur sur un serveur web)&#x20;
{% endhint %}

#### Applications en ligne de commande (chatbot)

[Ollama](https://ollama.com/), [llamafile](https://github.com/mozilla-ai/llamafile), [llama.cpp](https://github.com/ggml-org/llama.cpp)

#### Applications de bureau (chatbot + RAG)

[LM Studio](https://lmstudio.ai/), [Jan](https://www.jan.ai/), [GPT4All](https://www.nomic.ai/gpt4all), [AnythingLLM](https://anythingllm.com/), [Pleias ScholasticAI](https://github.com/Pleias/pleias_ScholasticAI)

#### Applications web (chatbot + RAG)

[Ollama chat](https://github.com/jakobhoeg/nextjs-ollama-llm-ui) : UI minimale sur une instance Ollama locale

[llama.cpp WebUI](https://github.com/ggml-org/llama.cpp/discussions/16938) : UI minimale sur une instance llama.cpp locale

[LocalAI](https://localai.io/)

[oobabooga text-generation-webui](https://github.com/oobabooga/text-generation-webui) : chatbot basé sur llama.cpp

#### Applications web RAG complètes (open source)

[Open WebUI](https://docs.openwebui.com/), [RAGFlow](https://ragflow.io/), [AperRAG](https://github.com/apecloud/ApeRAG),  [Ragapp](https://github.com/ragapp/ragapp), [Elysia](https://elysia.weaviate.io/), [RagFlow](https://ragflow.io/)











