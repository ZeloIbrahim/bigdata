# Big Data : TP1, TP2, TP3

OZEL Ibrahim (ID 26), 5A, cours Big Data. Rendu du 18/11/2026.

| TP | Sujet | Dossier | Notebook(s) |
|---|---|---|---|
| TP1 | Comptage de mots avec 4 méthodes : Python séquentiel, pseudo-MapReduce, Hadoop MapReduce, PySpark | [`TP1/`](TP1/README.md) | `TP_WordCount_BigData.ipynb` |
| TP2 | Accidents de la route avec PySpark (RDD, DataFrame, Spark SQL) | [`TP2/`](TP2/README.md) | `TP2_Road_Crash_PySpark.ipynb` |
| TP3 | Deux cas personnels : small data (prix des voitures en Pologne, régression) et big data (~50 000 images de vêtements, classification) | [`TP3_ID26/`](TP3_ID26/README.md) | `Small/TP3_Car_Prices_Poland.ipynb`, `Big/TP3_Outfit_Items_Images.ipynb`, `Big/TP3_Outfit_Items_Spark.ipynb` |

Chaque dossier a son propre README avec la consigne, les données, la façon de lancer le notebook et les résultats.

Dans les notebooks du TP2 et du TP3 :
- la première cellule indique la contribution : le notebook s'exécute-t-il en entier, les problèmes rencontrés et les modifications apportées ;
- chaque étape est précédée d'une cellule `## [Étape]` : Data, EDA, Prétraitement, Transformation, ML, Éval, Déploiement, Conclusion.

## Lancer

```bash
pip install numpy pandas matplotlib seaborn scikit-learn pyspark kagglehub   # + tensorflow pillow pour TP3/Big
jupyter notebook
```

Spark a besoin de Java 17 ou plus. Les données ne sont pas dans le dépôt (voir `.gitignore`) : chaque notebook les télécharge lui-même, depuis Kaggle ou depuis GitHub.
