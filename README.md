# Ciencia de Datos: phishing y enfermedad cardíaca

Trabajos prácticos de análisis de datos y Machine Learning realizados durante la carrera de Ingeniería en Informática (UBA).
Cada notebook se puede abrir y ejecutar directamente en Google Colab: los datos se descargan solos, sin iniciar sesión.

## Notebooks

| Notebook | Contenido | Tecnologías | Colab |
|---|---|---|---|
| [01 · Análisis exploratorio de phishing](notebooks/01_eda_phishing_pandas.ipynb) | EDA de 212.216 URLs, verificación de consistencia del dataset y visualizaciones | Pandas, Matplotlib, Seaborn | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Elias-Mendoza/ciencia-de-datos/blob/main/notebooks/01_eda_phishing_pandas.ipynb) |
| [02 · Phishing con Spark](notebooks/02_phishing_spark.ipynb) | Agregaciones distribuidas con RDDs (map / reduceByKey) sobre el mismo dataset | PySpark | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Elias-Mendoza/ciencia-de-datos/blob/main/notebooks/02_phishing_spark.ipynb) |
| [03 · Regresión Logística](notebooks/03_ml_regresion_logistica.ipynb) | Baseline: imputación, pipeline de preprocesamiento y búsqueda de regularización | scikit-learn | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Elias-Mendoza/ciencia-de-datos/blob/main/notebooks/03_ml_regresion_logistica.ipynb) |
| [04 · Random Forest](notebooks/04_ml_random_forest.ipynb) | Búsqueda de hiperparámetros e importancia de variables | scikit-learn | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Elias-Mendoza/ciencia-de-datos/blob/main/notebooks/04_ml_random_forest.ipynb) |
| [05 · XGBoost y entrega a Kaggle](notebooks/05_ml_xgboost_kaggle.ipynb) | Categóricas nativas, early stopping y generación del archivo de entrega | XGBoost | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Elias-Mendoza/ciencia-de-datos/blob/main/notebooks/05_ml_xgboost_kaggle.ipynb) |

## Parte 1: Detección de phishing (notebooks 01 y 02)

Dataset de 212.216 URLs etiquetadas como legítimas o de phishing, con features de la URL y del HTML de la página.

Hallazgos principales:

- El 42,8% de las URLs son de phishing.
- **Dígitos en la URL**: casi no aparecen en URLs legítimas (0,2% de los caracteres) y sí en las de phishing (6,3%).
  Cuando predominan los dígitos, el 97,5% de las URLs son phishing.
- **HTTPS** funciona como filtro negativo: prácticamente todas las páginas sin HTTPS son phishing,
  pero muchas páginas de phishing también usan HTTPS.
- Las páginas de phishing son mucho más simples: menos referencias, formularios y recursos.
  Entre las páginas con menos de 3 recursos (imágenes, CSS, JS), más del 93% son phishing.
- Las columnas de conteo de caracteres del dataset original no coinciden con un recálculo sobre la URL completa,
  así que el notebook 01 recalcula esas features.

## Parte 2: Predicción de enfermedad cardíaca (notebooks 03 a 05)

Clasificación binaria con 11 variables clínicas. La métrica pedida es accuracy; también se reportan
sensibilidad, especificidad y ROC-AUC.

Decisiones de metodología:

- **Colesterol faltante** (17,6% de los registros, cargado como 0): imputación por el promedio del grupo
  `(RestingECG, ChestPainType)`, con fallback a `RestingECG` cuando el grupo tiene menos de 10 datos,
  más una columna indicadora. En XGBoost se deja como `NaN` y el modelo lo maneja de forma nativa.
- **Sin data leakage**: todo el preprocesamiento está dentro de un `Pipeline` de scikit-learn y se ajusta
  solo con train en cada fold de validación cruzada.
- **Umbral de decisión** elegido con predicciones de validación cruzada, no sobre el conjunto de test.

## Cómo ejecutarlo

En Colab: botón "Open in Colab" de cada notebook y *Entorno de ejecución → Ejecutar todas*.

En local:

```bash
pip install -r requirements.txt
jupyter notebook
```

El notebook de Spark necesita Java 17 o superior.

## Autor

Elias Edgar Mendoza · [LinkedIn](https://linkedin.com/in/elias-mendoza-it) · [GitHub](https://github.com/Elias-Mendoza)
