# TP3 Big Data : chaîne complète de traitement sur deux datasets

OZEL Ibrahim (ID 26)

## Consigne

Prendre deux datasets, un « normal » et un « big », et dérouler sur chacun toute la chaîne :
**EDA → prétraitement → transformation → ML → évaluation**.
Les deux cas utilisent **PySpark** (consigne : compléter avec PySpark si le notebook d'origine ne l'utilise pas).

| Dossier | Dataset | Taille | Problème |
|---|---|---|---|
| [`Small/`](Small/README.md) | Car Prices Poland | 1 CSV, ~10 Mo | Régression : prix d'une voiture (scikit-learn puis PySpark MLlib) |
| [`Big/`](Big/README.md) | Outfit Items | ~49 900 images, ~6,5 Go | Classification d'images en 13 classes (Spark + TensorFlow, et version 100 % Spark) |

Chaque dossier contient ses notebooks, son propre dossier `data/` (ignoré par git, rempli automatiquement depuis Kaggle) et son README.

Dans chaque notebook, la première cellule donne la contribution (exécution complète, problèmes, modifications) et chaque étape est précédée d'une cellule `## [Étape]`.
