# TP1 Big Data : comptage de mots (WordCount)

Ozel Ibrahim

## Consigne

Compter les occurrences de chaque mot d'un fichier avec plusieurs méthodes, puis comparer leurs temps d'exécution sur des fichiers de plus en plus gros :

1. Python simple (un dictionnaire)
2. Pseudo-MapReduce en Python (map → shuffle → reduce sur une seule machine)
3. Hadoop MapReduce (job `wordcount` sur HDFS + YARN)
4. PySpark (bonus)

Tout est dans `TP_WordCount_BigData.ipynb`.

## Données

Le notebook les génère lui-même : un fichier de base de N = 500 mots tirés au hasard parmi 15, recopié m fois avec m = 1, 5, 50, 50 000 et 100 000. Ça va de 500 à 50 millions de lignes (315 Mo).
Les fichiers `mots_m*.txt` et `hadoop_m*.log` sont générés et ignorés par git. Les temps Hadoop sont enregistrés dans `temps_hadoop.csv`.

## Lancer

- Python : `pip install pandas matplotlib pyspark install-jdk`. Pour PySpark 4, il faut Java 17, que le notebook installe tout seul dans `~/.jdk`.
- Hadoop 3.4.0 : les commandes d'installation sont dans la section 4 du notebook. HDFS et YARN sont démarrés avec `--daemon start`, ce qui évite d'avoir besoin de SSH.

## Résultats (en secondes)

| Lignes | Python simple | Pseudo-MapReduce | Hadoop | PySpark |
|---:|---:|---:|---:|---:|
| 500 | 0,007 | 0,005 | 30,9 | 8,7 |
| 2 500 | 0,05 | 0,02 | 23,6 | 8,1 |
| 25 000 | 0,06 | 0,13 | 23,8 | 7,9 |
| 25 000 000 | 12,3 | 34,2 | 49,8 | 47,8 |
| 50 000 000 | 28,9 | 104,1 | 80,8 | 79,1 |

Les 4 méthodes donnent exactement les mêmes comptages. Pour Hadoop, la vérification est faite avec `sort | uniq -c` et `diff`.

## Conclusion

- **Python simple** est le plus rapide tant que les données tiennent en mémoire.
- **Pseudo-MapReduce** est environ 3 fois plus lent. Il montre bien la logique de MapReduce, mais sans plusieurs machines pour se répartir le travail, les étapes en plus ne font que coûter du temps.
- **Hadoop** a un coût fixe d'environ 24 s (JVM, YARN, HDFS) et écrit sur disque entre le map et le reduce. Il n'est intéressant que sur un cluster avec de très gros volumes.
- **PySpark** garde les données en mémoire. Son coût fixe est environ 3 fois plus faible que celui d'Hadoop, et c'est l'outil standard aujourd'hui pour les gros volumes.
