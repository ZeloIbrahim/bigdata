# TP2 Big Data : accidents de la route avec PySpark

Ozel Ibrahim

## Consigne

Refaire l'étude « Big Data Analysis of Road Crash Data using PySpark » de Rakesh Nain (Medium) en utilisant les trois API de Spark : **RDD**, **DataFrame** et **Spark SQL**. On compare ensuite ces API, avec les résultats et les temps d'exécution.

Tout est dans `TP2_Road_Crash_PySpark.ipynb`.

## Données

Accidents de la route en Australie-Méridionale entre 2012 et 2019 (DPTI, data.sa.gov.au), environ 77 Mo :
- `data/crash_data/` : une ligne par accident, 127 672 lignes ;
- `data/units_data/` : une ligne par véhicule ou piéton impliqué, 272 591 lignes.

Les deux tables se relient par `REPORT_ID`. La description des colonnes est dans `data/metadata-for-road-crash.pdf`.
Le notebook télécharge les CSV depuis le GitHub de l'auteur s'ils ne sont pas déjà là (`TP2/data/` est ignoré par git).

## Lancer

```bash
pip install pyspark pandas matplotlib   # Java 17+ requis
jupyter notebook TP2_Road_Crash_PySpark.ipynb
```

Spark tourne en `local[*]`, c'est-à-dire sur tous les cœurs de la machine.

## Contenu

1. **RDD** : `parallelize` / `textFile`, parsing CSV, partitionnement personnalisé (SA dans une partition, les autres États dans l'autre), âge moyen par sexe, véhicules les plus anciens et les plus récents.
2. **DataFrame** : accidents à Adelaide avec plus de 3 victimes, top 10 des accidents par nombre de victimes, nombre de morts par type d'accident, victimes par quartier quand le conducteur n'avait pas de permis (jointure entre les deux tables), gravité des accidents selon l'alcool et la drogue (une version avec UDF et une avec les fonctions natives de Spark), graphiques.
3. **Comparaison RDD / DataFrame / SQL** : deux requêtes écrites des trois façons. On vérifie que les résultats sont identiques et on mesure les temps.

## Résultats principaux

- Environ 80 % des véhicules sont immatriculés en SA. L'âge moyen des conducteurs est d'environ 40 ans, pour les hommes comme pour les femmes.
- Les chocs contre un obstacle fixe sont les accidents qui font le plus de morts.
- Part d'accidents mortels : 0,4 % sans alcool ni drogue, 3,4 % avec alcool, 6,3 % avec drogue, 13,1 % avec les deux.
- Temps d'exécution (s) :

| Requête | RDD | DataFrame | Spark SQL |
|---|---:|---:|---:|
| Accidents à Adelaide | 5,61 | 1,33 | 1,26 |
| Victimes sans permis par quartier | 4,35 | 1,09 | 1,29 |

Les DataFrames et Spark SQL passent par le même optimiseur, donc leurs temps sont proches. Les RDD sont environ 4 fois plus lents, parce que chaque ligne passe par Python et que Spark ne peut pas optimiser la requête.
