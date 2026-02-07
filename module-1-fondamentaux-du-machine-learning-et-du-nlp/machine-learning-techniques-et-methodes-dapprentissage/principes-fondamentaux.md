# Principes fondamentaux

Le Machine Learning repose, tout comme l'IA en général, sur une **démarche inductive et data-centric**, or ce paradigme est en grande partie étranger à la culture de la donnée (plutôt déductive) telle qu'elle est pratiquée en bibliothèques , _ce qui peut expliquer leur difficulté à appréhender l'IA générative aujourd’hui._

Le principe de base de l'**apprentissage machine à partir de grands volumes de données** est de considérer que, sous certaines conditions, les données recèlent des corrélations, des relations et des associations qui échappent à l'observation humaine avec des outils standards comme les tableurs. Ces **patterns sous-jacents** et signaux faibles peuvent être identifiés et capturés par des algorithmes statistiques et mathématiques dans le but de réaliser des **prédictions.**

## Logique

### De la règle métier (logique déductive) à l'approche data-centric (logique inductive)

{% hint style="info" %}
Logique déductive : on formalise des décisions à partir de règles connues et déterministes, appliquées systématiquement aux données
{% endhint %}

{% hint style="info" %}
Logique inductive : apprentissage à partir de grandes quantités de données
{% endhint %}

{% columns %}
{% column %}
**Logique déductive : On définit des règles explicites basées sur notre connaissance du métier, nos procédures**

* Désherbage : appliquer manuellement un système de règles (filtres successifs) sur un fichiers de données d'exemplaires
* Contröle-qualité des données : développer des systèmes-experts à partir de règles logiques
* Si l’usager a rendu trois livres en retard, alors il est suspendu


{% endcolumn %}

{% column %}
**Logique inductive : on observe des exemples (des faits, des données) et on en infère des modèles.**

* Prédire les exemplaires à desherber en inférant les règles à partir de l'analyse du fichier &#x20;

&#x20;                                           &#x20;

* Laisser un modèle pré-entraîné détecter les données à corriger
* Prédire la probabilité qu’un usager rende un livre en retard en analysant les historiques de prêts
{% endcolumn %}
{% endcolumns %}

### Implications pratiques

* Il ne s’agit pas de tout automatiser ou remplacer les humains par des modèles, mais de **changer, là où c'est pertinent, la manière dont on prend certaines décisions**.
* Avant d'appliquer un algorithme sur un jeux de données, celui-ci doit être préparé et structuré pour que les méthodes statistiques agissent de manière pertinente : la phase de pré-traitement (nettoyage, datavisualisation..) est essentielle et permet d'analyser en profondeur et comprendre ses données&#x20;

## ML : typologie des algorithmes

{% hint style="info" %}
Algorithme : ensemble de règles, procédures et méthodes statistiques
{% endhint %}

* Algorithmes supervisés : le jeu de données d’entraînement est dit étiquetté, c’est-à dire que la variable cible à prédire fait partie du dataset de départ et que l’algorithme apprend en mappant les inputs (les valeurs des variables prédictives) sur les outputs (les valeurs de la variables cible)
* Algorithmes non supervisés : le jeu de données d’entraînement n’est pas étiquetté, c’est-à dire que la variable à prédire ne fait pas partie des données d’apprentissage et que l’algorithme apprend seul par exploration de la structure des données
* Algorithmes de régression : la variable de sortie à prédire est un nombre
* Algorithmes de classification : la variable de sortie à prédire est une catégorie
* Algorithme probabiliste : l'algorithme modélise les relations entre variables en utilisant des concepts de probabilités et de statistiques pour faire ses prédictions  (ex : calcul de la probabilité d'appartenance à une classe, estimation d'une distribution de probabilité)
* Algorithme géométrique : l'algorithme s'appuie sur des notions géométrique de distance, de proximité ou de séparation dans l'espace des variables à partir de la position relative des points de données pour effectuer ses prédictions.&#x20;

| Type                    | Algorithmes                                                                                                                                                                                                                                                                       |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Régression              | <ul><li>Régression linéaire (supervisé / géométrique)</li><li>Régression polynomiale (supervisé / probabiliste)</li><li>Descente de gradiant</li><li>Machines à vecteurs de support - SVM (supervisé / géométrique)</li><li>Arbre de décision</li><li>Forêts aléatoires</li></ul> |
| Classification          | <ul><li>Régression logistique</li><li>Machines à vecteurs de support - SVM (supervisé / géométrique)</li><li>Arbre de décision</li><li>Forêts aléatoires</li></ul>                                                                                                                |
| Réduction de dimensions | <ul><li>Analyse en composantes principales (PCA)</li><li>Plongement localement linéaire (LLE)</li></ul>                                                                                                                                                                           |
| Clusterisation          | <ul><li>k-means (non supervié / géométrique)</li><li>DBSCAN (non supervié / géométrique)</li></ul>                                                                                                                                                                                |

## Modèle de ML

<figure><img src="../../.gitbook/assets/Capture d&#x27;écran 2025-07-16 113224.png" alt=""><figcaption></figcaption></figure>
