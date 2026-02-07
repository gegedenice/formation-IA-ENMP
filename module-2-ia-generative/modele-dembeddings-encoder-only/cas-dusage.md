# Cas d'usage

## Cartographie

{% embed url="https://atlas.nomic.ai/data/geraldine.geoffroy/enmp-openalex-dataset/map/8d7a8d8f-f8b2-4a3e-88f0-d75cb2e9b16b#jmmy" %}

* Encodage de textes en vecteurs
* Réduction de dimension : des algorithmes de réduction de dimension (PCA, UMAP, t-SNE) permettent de réduire les dimensions des vecteurs en préservant les relations de proximité locale
* Projection des vecteurs 2D ou 3D
* Clusterisation "automatique"
  * Colorisation par clusters (ou métadonnées)
  * Eventuellement : topic modeling (extraction de mots-clés pertinents)

### Applications

* Dataviz : point d'entrée "sémantique" dans une collection
* Clusters -> identification des thématiques
* Détection d'ouliers
* Détection de paraphrases

## Calculs de similarités -> Recherche conceptuelle (au-delà des mots-clés exacts)

Comme il est matématiquement possible de calculer des distances en deux vecteurs (distance cosinus, distance euclidienne, produit scalaire...), il est possible d'effectuer des calculs de similarités entre paires de vecteurs et ainsi de transformer une requête textuelle en une recherche par proximité dans l'espace vectoriel, permettant d'interroger une base de données d'embeddings pré-calculés qui retournent les documents les plus pertinents sémantiquement.

### Applications liées à l'appariement de notices similaires

* Recommandations
* Moteur de recherche sémantique
* FRBRisation

## Extraction d'entités nommées NER



### Applications

* Indexation automatisée
* Indexation + alignements (NER + NEL)
* NER ciblée (sur DOI, ISBN, Orcid...)
* NER pour détection d'afiliation à partir d'adresses

## Expansion de requêtes contrôlée (Par exemple à partir d'un référentiel ou d'une liste d'autorités)

* On convertit en embeddings tous les termes de la liste d'autorités
* requête q embeddée : on cherche ses k plus proches voisins dans l’index des termes
* Filtres "contrôlés" : on ne retient par exemple que les termes les plus proches du même cluster d’embeddings

Exemple :  `droit d'auteur` → {`copyright`, `licence open access` (rejeter: trop éloigné), `propriété intellectuelle` (si voisin réciproque)} -> ("droit d'auteur"^3) OR (copyright^2) OR ("propriété intellectuelle"^1.5)

## Classification

Plusieurs solutions techniques sont envisageables

* Supervisé : encodage des séquences en embeddings + application d'un algorithe "classique" de ML supervisé sur le corpus embeddé et déjà classifié
* Non supervisé : encodage des séquences en embeddings + clusterisation + topic modeling (avec BertTopic par exemple) + projection de la query embeddée
* Supervisé : on représente chaque catégorie par l'embeddings de toutes les séquences préalablement classifiées + recherche sémantique à partir de la query embeddée (cf [https://abes.fr/wp-content/uploads/2025/04/etude-experimentation-indexation-rameau-assistee-par-ia.pdf](https://abes.fr/wp-content/uploads/2025/04/etude-experimentation-indexation-rameau-assistee-par-ia.pdf))

















[https://www.istex.fr/homogeneisez-vos-mots-cles-anglais-grace-au-web-service-datahomogenise/](https://www.istex.fr/homogeneisez-vos-mots-cles-anglais-grace-au-web-service-datahomogenise/)
