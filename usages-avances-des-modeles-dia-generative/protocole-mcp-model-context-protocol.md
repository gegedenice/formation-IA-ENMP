# Protocole MCP (Model Context Protocol)

{% hint style="info" %}
Le protocole MCP peut être vu comme une extension et une standardisation du concept de tools.
{% endhint %}

Le MCP est un protocole universel standardisé et ouvert (développé par Anthropic) conçu pour l'**interopérabilité des outils d'IA** qui permet de normaliser l'outillage des LLM en le connectant à des serveurs d’outils qui exposent des données, des fichiers, des états d'observation ou des modalités d'action externes.&#x20;

Ce protocole permet donc à tout à chacun de créer des serveurs indépendants des modèles ayant vocation à être intégrés selon une **logique modulaire plug-and-play** durant le processus génératif du LLM afin que celui-ci puisse interagir avec des services ou des sources de données externes.

Tout LLM compatible MCP peut être intégré dans un client MCP et interroger n’importe quel serveur MCP, et c’est cette généricité permettant de s’abstraire d’intégrations techniques potentiellement complexes avec chaque source de données différente qui rend ce protocole particulièrement utilisé.

Côté développeur, exposer un service ou des données avec un serveur MCP permet d’interagir avec ce service ou ces données en langage naturel avec un LLM qui analyse la requête utilisateur, identifie les outils à appeler et avec quels paramètres (via les schémas fournis par les serveurs MCP), génère les appels de fonctions puis, après retour de la réponse, la reformate pour la restituer de manière intelligible.

## Registres de serveurs MCP

Pour l'instant il n'existe pas de registre officiel de référence, mais plusieurs listes de serveurs sont disponibles

* [https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
* [https://github.com/mcp](https://github.com/mcp)
* [https://www.pulsemcp.com/servers](https://www.pulsemcp.com/servers)

### Exemples de serveurs MCP

Computer use (automatisation de tâches dans le navigateur) : [Browser Automation MCP](https://docs.browser-use.com/customize/integrations/mcp-server)

Accès aux fichiers locaux : [Filesystem MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)

Scraping de page web : [Fetch MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)

### Installation
