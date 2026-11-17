# TP3 Dataset « normal » : prix des voitures en Pologne

Ozel Ibrahim

Notebook : `TP3_Car_Prices_Poland.ipynb`

## Données

[Car Prices Poland (Kaggle)](https://www.kaggle.com/datasets/aleksandrglotov/car-prices-poland) : un seul CSV d'environ 10 Mo, avec 117 927 annonces de janvier 2022 et des prix en złotys (PLN).
Il s'agit d'une **régression** : prédire `price` à partir de la marque, du modèle, de l'année, du kilométrage, du moteur, du carburant et de la région.

Si le fichier `data/Car_Prices_Poland_Kaggle.csv` n'existe pas, le notebook le télécharge avec `kagglehub`.

## Lancer

```bash
pip install numpy pandas matplotlib seaborn scikit-learn pyspark kagglehub   # Java 17+ pour PySpark 4
jupyter notebook TP3_Car_Prices_Poland.ipynb
```

## Chaîne de traitement

- **EDA** : le prix est très asymétrique (skew de 3,67, ramené à 0,12 avec le log). Il y a 6 473 doublons, `generation_name` manque pour 30 085 lignes et `city` a 4 427 valeurs différentes.
- **Prétraitement** : on supprime les doublons et les valeurs aberrantes (quantiles 0,5 % à 99,5 %). Il reste 109 509 lignes.
- **Transformation** : ajout de `age` et `km_par_an`, one-hot pour `fuel` et `province`, target encoding pour `mark`, `model` et `generation_name`, standardisation, cible en `log(1 + prix)`. Le tout est dans un `Pipeline` scikit-learn.
- **ML** : Ridge, Random Forest, HistGradientBoosting.
- **Évaluation** : MAE, RMSE, MAPE, R², validation croisée à 5 plis, importance par permutation.
- **Version PySpark** (section 7) : la même chaîne avec un DataFrame Spark (agrégations distribuées, `approxQuantile`, target encoding par `groupBy` + `broadcast`) et Spark MLlib (LinearRegression, RandomForest, GBT), puis une comparaison avec scikit-learn.

## Résultat

Meilleur modèle : **Random Forest, R² = 0,948 ± 0,002** en validation croisée. L'erreur médiane va de 8 à 13 % selon la tranche de prix.

| R² sur le jeu de test | Linéaire | Random Forest | Boosting |
|---|---:|---:|---:|
| scikit-learn | 0,860 | 0,950 | 0,946 |
| PySpark MLlib | 0,855 | 0,930 | 0,921 |

Spark retrouve le même classement des modèles. Il est un peu moins précis (arbres limités en profondeur, variables découpées en intervalles) et plus lent à cette échelle, mais le même code passe à l'échelle.
