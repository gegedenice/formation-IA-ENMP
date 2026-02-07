# Tools : les LLM comme systèmes d'action

{% hint style="warning" %}
Tous les LLM ne sont pas compatibles avec l'usage de tools (et MCP) durant l'inférence, ils doivent avoir été spécifiquement entraînés et configurés pour cela.
{% endhint %}

{% hint style="info" %}
Ressources

* UI avec exemples de tools :[https://huggingface.co/spaces/Nymbo/Tools](https://huggingface.co/spaces/Nymbo/Tools)
* Exemple de registre de tools (communautaires) utilisables avec le framework Llamaindex : [https://llamahub.ai/?tab=tools](https://llamahub.ai/?tab=tools)
{% endhint %}

Les LLM sont puissants mais sujets à des limites internes inhérentes à leur archirtecture :

* Système statique : connaissance figée par le corpus de pre-training
* Système fermé sans accès au "monde extérieur" :&#x20;
  * pas d’accès Internet,
  * pas d’accès à des fichiers,
  * pas d’accès à des bases de données ou à des APIs métier,
  * pas de consultation de documents stockés localement.
* Système génératif : pas d'action, seulement de la prédiction de texte. un LLM ne peut ni exécuter du code, ni lancer des scripts, ni agir sur un système

**Les tools ou "function calling" permettent de dépasser ces limitations et d'étendre les capacités des modèles au-delà de leurs connaissances internes en leur donnant la possibilité d'invoquer des fonctions externes venant enrichir leurs réponses en automatisant des recherches, des analyses, des accès à des bases ou SI externes...**

**Exemple simple de LLM avec accès au web dans l'écosystème OpenAI (tool web\_search natif)**

<pre><code><strong>prompt = """
</strong>Trouve le ppn de l'autorité personne Albert Camus dans le référentiel français Idref.
"""
response = openai_client.responses.create(
    model="gpt-5",
    input=prompt,
    #instructions="",
    tools=[{"type": "web_search"}],
    reasoning={ "effort": "high" }, #minimal|low|medium|high
    text={ "verbosity": "medium" }, #low|medium|high
    stream=False,
)

#Réponse
PPN IdRef (autorité personne) d’Albert Camus: 026764644. (idref.fr)

🔎 Requêtes web_search effectuées
1. IdRef Albert Camus PPN
id: ws_0cd9e1f6a985e4e800691c2f2234048194bc196579ab695b31 — statut: completed

2. site:idref.fr Camus , Albert (1913-1960) Personne
id: ws_0cd9e1f6a985e4e800691c2f251400819489f4ef5db328c388 — statut: completed

3. —
id: ws_0cd9e1f6a985e4e800691c2f3233308194b407c31c86089930 — statut: completed
</code></pre>

Les écosystèmes programmatiques permettant d'implémenter des tools chacun avec ses propres mécanismes sont très nombreux (OpenAI, librairie HuggingFace transformers, frameworks LangChain ou LlamaIndex...) , mais le principe général reste identique :

{% stepper %}
{% step %}
Un LLM reçoit une question
{% endstep %}

{% step %}
Le LLM décide d’appeler un tool
{% endstep %}

{% step %}
Le tool retourne un résultat&#x20;
{% endstep %}

{% step %}
Le modèle l’intègre dans sa réponse
{% endstep %}
{% endstepper %}

**Exemple de LLM avec accès à Arxiv dans l'écosystème LLamaIndex**

```
#Exemple de LLM avec accès à Arxiv dans l'écosystème LLamaindex

arxiv_tool = ArxivToolSpec()
agent = FunctionAgent(
    tools=arxiv_tool.to_tool_list(),
    llm=OpenAI(model="gpt-4.1")
)
await agent.run("Recent research papers about superconductor lk-99")

#Réponse
1. Effects of the Next-Nearest-Neighbor Hopping on the Low-Dimensional Hubbard Model: Ferromagnetism, Antiferromagnetism, and Superconductivity
   - This paper reviews recent progress in understanding competing phases (including superconductivity) in low-dimensional systems, which are relevant for high-critical-temperature superconductors and novel quantum materials.
   - [Read the paper](https://arxiv.org/pdf/2511.13534v1)

2. Tuning of Weyl point emergence in multi-terminal Josephson junctions using quantum point contacts
   - This study investigates the emergence of exotic electronic states (including superconductivity) in multi-terminal Josephson junctions, which are important for understanding new superconducting materials.
   - [Read the paper](https://arxiv.org/pdf/2511.13493v1)

3. Spin-Lattice Relaxation in Two-Dimensional Superconducting BKT Transition
   - This paper discusses experimental signatures of the Berezinskii-Kosterlitz-Thouless transition in two-dimensional superconductors, which is relevant for unconventional superconductivity research.
   - [Read the paper](https://arxiv.org/pdf/2511.13486v1)
```

{% hint style="success" %}
**Flexibilité du concept** : quel que soit le framework ou l'environnement, les tools sont des briques extrèmement flexibles et personnalisables, et n’importe quel composant externe peut devenir un tool (un RAG, un modèle de vision, une API métier, un code système, una application Gradio dans un space HuggingFace...)
{% endhint %}
