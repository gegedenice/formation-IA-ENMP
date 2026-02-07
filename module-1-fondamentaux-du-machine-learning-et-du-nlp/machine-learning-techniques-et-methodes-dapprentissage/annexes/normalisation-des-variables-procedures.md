# Normalisation des variables : procédures

## **Procédure normale de normalisation (standardisation) des variables**

#### Étapes standards dans un pipeline Machine Learning

1. **Identifier les variables numériques continues**
   * Exemples : `age_ex`, `nb_emprunts`, `pages`, etc.
2. **Séparer train / test**
   * Important pour éviter le **data leakage**
3.  **Appliquer le scaler sur le training set uniquement**

    * `StandardScaler` (centrer/réduire)
    * ou `MinMaxScaler` (entre 0 et 1)

    ```python
    pythonCopierModifierscaler = StandardScaler()
    X_train_scaled = scaler.fit_transform(X_train)
    X_test_scaled = scaler.transform(X_test)
    ```
4. **Les variables catégorielles ou ordinales** sont encodées à part (OneHot, OrdinalEncoder, etc.), **non normalisées.**

{% hint style="info" %}
Le scaling ne supprime pas les pics, les asymétries ou les concentrations dans une distribution. Il **transforme l’échelle**, pas la forme. Une fréquence élevée dans la distribution de départ restera visible après scaling, elle deviendra "juste" un pic à une certaine valeur standardisée&#x20;
{% endhint %}

## Cas spécifiques où la normalisation est inadaptée

| Situation                                                                                          | Solution                                                         |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Valeur aberrante (outlier)**                                                                     | Neutraliser (remplacer, winsoriser, binariser) avant de scaler   |
| **Pic artificiel lié à une migration ou un bug**                                                   | Corriger (binning, interpolation, suppression) avant le scaling  |
| **Distribution très asymétrique (skewed)**                                                         | Envisager une transformation (log, Box-Cox) avant scaling        |
| **Variable numérique mais sémantiquement discrète** (ex: tranches d’âges ou années de publication) | Ne pas normaliser, mais **transformer en catégories**            |
| **Variable qui contient une codification métier** (ex: cotes Dewey, niveaux MARC)                  | Ne pas normaliser, **encodage catégoriel** adapté au sens métier |
