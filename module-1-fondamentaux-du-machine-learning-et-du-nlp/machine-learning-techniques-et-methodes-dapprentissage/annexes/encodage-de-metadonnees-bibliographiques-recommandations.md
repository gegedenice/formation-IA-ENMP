# Encodage de métadonnées bibliographiques : recommandations

| Champ (métadonnée)                              | Type brut               | Type pour ML             | Encodage recommandé                                                 | Remarques et pièges                                    |
| ----------------------------------------------- | ----------------------- | ------------------------ | ------------------------------------------------------------------- | ------------------------------------------------------ |
| **Titre**                                       | Texte libre             | Texte / identifiant      | ⚠️ À exclure ou vectoriser (TF-IDF, embeddings)                     | Trop spécifique, très bruité, sensible à la langue     |
| **Auteur principal**                            | Texte (nom)             | Catégorique (nominal)    | Regrouper auteurs fréquents / top N, puis one-hot ou label encoding | Trop de modalités, long tail problématique             |
| **Cote Dewey**                                  | Chaîne pseudo-numérique | Catégorique hiérarchique | Extraire 2 ou 3 premiers chiffres, puis one-hot ou ordinal encodé   | ⚠️ Ne pas traiter comme un nombre continu              |
| **Année de publication**                        | Entier (`YYYY`)         | Numérique ordinale       | Garder tel quel ou transformer en `Âge`                             | À normaliser ; peut refléter l’obsolescence            |
| **Langue du document**                          | Code ISO / texte        | Catégorique nominale     | One-hot encoding (si peu de langues)                                | Souvent peu de classes utiles                          |
| **Type de document** (monographie, thèse, etc.) | Catégorique texte       | Nominal                  | One-hot encoding ou regroupement thématique                         | Fréquemment discriminant                               |
| **Éditeur**                                     | Texte libre             | Catégorique nominale     | Regrouper les plus fréquents, puis encodage                         | Peu pertinent seul, mais indicateur sectoriel possible |
| **Sujet(s) RAMEAU**                             | Liste de termes         | Multi-label              | Extraction des concepts, encodage binaire multi-hot                 | Peut servir à modéliser les thèmes dominants           |
| **Nombre de pages**                             | Entier                  | Numérique continue       | Normalisation (standard/scaler)                                     | Parfois informatif : thèse vs roman                    |
| **Date d’ajout au catalogue**                   | Date                    | Temporel                 | Extraire `année` ou `ancienneté`, puis normaliser                   | Reflet des pratiques de collection                     |
| **Date du dernier prêt**                        | Date                    | Temporel / usage         | Transformer en `jours depuis dernier prêt`                          | Très utile pour prédire le non-usage                   |
| **Nombre d’exemplaires**                        | Entier                  | Numérique discrete       | Tel quel ou normalisé                                               | Parfois indicateur de diffusion ou d’importance        |
| **Localisation** (salle, BU, etc.)              | Catégorique             | Nominal                  | One-hot ou regroupement                                             | Peut refléter une politique documentaire différenciée  |
| **Public cible** (jeunesse, adulte…)            | Catégorique             | Nominal                  | One-hot encoding                                                    | Fortement lié aux usages                               |

De manière générale&#x20;

* **Éviter de traiter des chaînes numériques comme des quantités** (cote Dewey, ISBN, etc.).

{% hint style="info" %}
Même si une donnée ressemble à un nombre, il faut toujours se demander si l'on peut **calculer une moyenne ou une distance entre deux valeurs ?** Si non, c’est une catégorie déguisée.
{% endhint %}

* **Limiter les catégories trop nombreuses** : regrouper (`Top N`, `autres`) pour auteurs, éditeurs.
* **Préférer les variables dérivées** (âge du document, délai depuis dernier prêt) aux dates brutes.
* **One-hot encoding** si peu de modalités ; sinon, passer à des modèles qui supportent des catégories non encodées (arbres, gradient boosting).
* Pour les textes longs (titre, résumé), envisager TF-IDF ou embeddings si  NLP, mais pas forécement recommandé dans un modèle supervisé classique.
