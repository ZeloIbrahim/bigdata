# TP3 Dataset « big » : images de vêtements (Outfit Items)

Ozel Ibrahim

Notebooks :
- `TP3_Outfit_Items_Images.ipynb` : version TensorFlow (`tf.data` + fine-tuning de MobileNetV2) ;
- `TP3_Outfit_Items_Spark.ipynb` : version **PySpark**, où chaque étape est distribuée par Spark (liste des chemins lue par les tâches Spark, EDA et doublons par hash, extraction de caractéristiques MobileNetV2 dans une `pandas_udf`, régression logistique Spark MLlib).

## Données

[Outfit Items (Kaggle)](https://www.kaggle.com/datasets/kritanjalijain/outfititems) : environ 49 900 images PNG, soit 6,5 Go, réparties en 5 catégories et 13 sous-catégories.
Il s'agit d'une **classification d'images** en 13 classes.

Si `data/outfit/` est vide, le notebook télécharge les images avec `kagglehub` (environ 7 Go, une seule fois).

**Pourquoi c'est du big data :** une fois décodées, les images ne tiennent pas en mémoire. On ne charge donc jamais tout le dataset : les images sont lues, décodées et transformées en flux, par lots et en parallèle, avec `tf.data`.

## Lancer

```bash
pip install numpy pandas matplotlib seaborn scikit-learn tensorflow pillow kagglehub
jupyter notebook TP3_Outfit_Items_Images.ipynb
```

Sur CPU, l'entraînement est long. Pour aller plus vite, on peut baisser `EPOCHS_TETE` et `EPOCHS_FINE`.

Pour la version PySpark : Java 17 ou plus est nécessaire (le notebook prend automatiquement le JDK de `~/.jdks` s'il existe). `IMAGES_PAR_CLASSE` limite le nombre d'images utilisées pour l'apprentissage.

## Chaîne de traitement

- **EDA** : classes déséquilibrées, tailles d'image et modes couleur variables (RGB, RGBA, gris), analysés sur un échantillon de 1 000 images.
- **Prétraitement** : on vérifie en parallèle que chaque fichier se décode, on numérote les classes, on fait un découpage train/validation/test stratifié et on calcule des poids de classe.
- **Transformation** : conversion en RGB, redimensionnement à 160×160 avec padding, normalisation, augmentation (miroir, rotation, zoom, contraste), traitement parallèle et `prefetch`.
- **ML** : transfer learning avec MobileNetV2 pré-entraîné sur ImageNet. On entraîne d'abord la tête seule, puis on fait un fine-tuning des 30 dernières couches, avec `EarlyStopping`.
- **Évaluation** : accuracy, F1 macro, rapport par classe, matrice de confusion, accuracy sur les 5 grandes catégories, exemples d'erreurs. Le modèle est sauvegardé dans `outfit_mobilenetv2.keras` (ignoré par git).
