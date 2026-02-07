# Encodage des dates : table de décision

| Variable (exemples fréquents)                   | Type brut               | Traitement recommandé                                         | Encodage final (int / one-hot / feature dérivée)       | Remarques métiers / IA                                                                                      |
| ----------------------------------------------- | ----------------------- | ------------------------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| **Année de publication** (`YYYY`)               | Date (année seule)      | **Numérique continue** ou **transformée en âge**              | `int` → normalisé (ou `Age_document = 2025 - YYYY`)    | Indique l’ancienneté d’un document. Plus utile comme progression temporelle.                                |
| **Date d'ajout au catalogue**                   | Date complète           | Extraire : année, mois, jour, ou **ancienneté**               | `Age_ajout = 2025 - année d’ajout` (puis normalisé)    | Peut refléter des évolutions de politique documentaire ou de pratiques d’indexation.                        |
| **Date du dernier prêt**                        | Date complète           | Calculer : **délai depuis dernier prêt**                      | `int` (ex : jours ou mois depuis dernière utilisation) | Utile comme indicateur d’obsolescence ou de non-usage.                                                      |
| **Année de thèse, événement, réforme**          | Date (année symbolique) | **Catégorique ou discrétisée** (ex : avant/après une réforme) | One-hot ou booléen (ex : `Post-Réforme 2000`)          | Si des changements majeurs (juridiques, pédagogiques…) sont connus.                                         |
| **Mois de publication**                         | Mois (1 à 12)           | **Cyclic encoding** ou **one-hot**                            | sin/cos transform ou one-hot                           | Mois peut refléter une saisonnalité éditoriale, mais rarement pertinent seul.                               |
| **Jour de la semaine de prêt**                  | Jour (0 = lundi)        | **Cyclic encoding**                                           | sin/cos                                                | Pour prédire des habitudes d’usage ou de circulation. Rarement utile dans le contexte de métadonnées pures. |
| **Période historique ou littéraire** (si codée) | Catégorie textuelle     | Coder comme **catégorie nominale**                            | One-hot ou label encoding                              | Ex : « XIXe siècle », « Renaissance » → à conserver en tant que telle.                                      |

### Encodages spéciaux

| Situation                               | Encodage recommandé                                  | Pourquoi ?                                                                               |
| --------------------------------------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Mois, jours, heures (valeurs cycliques) | **Cyclic encoding** avec sin/cos                     | Car janvier et décembre sont proches (1 et 12 ne doivent pas être vus comme “éloignés”). |
| Date complète inutile dans son format   | Extraire des composantes utiles (année, mois, délai) | Les modèles ne comprennent pas `datetime` nativement.                                    |

### Exemples de code

```
from datetime import datetime

df["Age_document"] = 2025 - df["Date Publication"]
df["Mois_ajout"] = pd.to_datetime(df["Date Ajout"]).dt.month
df["sin_mois"] = np.sin(2 * np.pi * df["Mois_ajout"]/12)
df["cos_mois"] = np.cos(2 * np.pi * df["Mois_ajout"]/12)
df["Depuis_dernier_prêt"] = (datetime.today() - pd.to_datetime(df["Date Dernier Prêt"])).dt.days

```
