# TFG — Detección de riesgo crediticio con modelos interpretables

Este repositorio contiene el desarrollo experimental de un Trabajo Fin de Grado centrado en la aplicación de técnicas de *machine learning* interpretables al análisis de riesgo crediticio.

El proyecto se inspira en una tesis doctoral sobre *machine learning* interpretable para la detección del fraude crediticio, pero no pretende replicarla por completo. El objetivo es adaptar parte de su enfoque metodológico a un alcance realista de TFG, priorizando la claridad, la trazabilidad experimental y la interpretación de resultados.

## Estado actual del repositorio

Actualmente el repositorio contiene el notebook:

```text
02_german_credit_feature_selection.ipynb
```

Este notebook está completo y cerrado. En él se desarrolla un experimento de selección de variables sobre el dataset **German Credit**, comparando distintas representaciones de los datos y evaluando el equilibrio entre rendimiento predictivo, reducción de dimensionalidad e interpretabilidad.

El resto de notebooks del proyecto se irán incorporando progresivamente.

## Objetivo del proyecto

El objetivo general del TFG es analizar cómo distintas estrategias de representación y selección de variables afectan al rendimiento y a la interpretabilidad de modelos de clasificación aplicados a riesgo crediticio.

De forma concreta, el notebook actual busca responder a las siguientes preguntas:

* ¿Es posible reducir la dimensionalidad del dataset sin perder demasiado rendimiento?
* ¿Qué variables son más relevantes para distinguir entre buen y mal riesgo crediticio?
* ¿Qué modelos ofrecen mejor equilibrio entre rendimiento e interpretabilidad?
* ¿Qué representación de los datos resulta más adecuada para continuar con un autoencoder en el siguiente experimento?

## Dataset utilizado

El notebook utiliza el dataset **Statlog German Credit Data**, disponible a través de UCI Machine Learning Repository.

El dataset contiene 1000 observaciones y 20 variables descriptivas relacionadas con solicitudes de crédito. La variable objetivo distingue entre:

* buen riesgo crediticio;
* mal riesgo crediticio.

En el notebook, la variable objetivo se recodifica como:

```text
0 = buen riesgo
1 = mal riesgo
```

## Metodología del notebook actual

El notebook `02_german_credit_feature_selection.ipynb` sigue las siguientes fases:

1. Carga y preparación semántica del dataset.
2. Análisis descriptivo inicial.
3. División estratificada en entrenamiento y test.
4. Construcción de una Variante A basada en One-Hot Encoding completo.
5. Evaluación de modelos clásicos:

   * Logistic Regression;
   * SVC lineal;
   * Linear Discriminant Analysis;
   * Gradient Boosting;
   * Random Forest.
6. Cálculo de importancias mediante varios criterios:

   * coeficientes de modelos lineales;
   * importancias de modelos basados en árboles;
   * información mutua;
   * criterio CME simplificado.
7. Construcción de subconjuntos de variables:

   * IVI-like;
   * MIFF-like;
   * RFF-like.
8. Análisis de interpretabilidad de la selección principal.
9. Diseño e implementación de una Variante B con codificación mixta.
10. Comparación entre Variante A y Variante B.
11. Evaluación final en test.
12. Selección de la entrada para el futuro autoencoder.
13. Exportación de artefactos y cierre reproducible del notebook.

## Representaciones evaluadas

Se comparan dos estrategias de codificación.

### Variante A — One-Hot Encoding completo

Todas las variables categóricas se codifican mediante One-Hot Encoding. Esta variante genera una representación inicial de 61 variables.

A partir de la selección MIFF-like, se reduce la representación a 26 variables.

### Variante B — Codificación mixta

La Variante B utiliza una codificación adaptada al significado de cada variable:

* variables numéricas estandarizadas;
* variables nominales mediante One-Hot Encoding;
* variables binarias como indicadores 0/1;
* variables con estructura ordinal parcial mediante nivel ordinal más indicador específico.

Esta variante genera una representación inicial de 51 variables. Su subconjunto MIFF-like contiene 29 variables.

## Resultados principales

La representación principal seleccionada para continuar el proyecto es:

```text
Variante A — OHE + MIFF-like
```

Esta representación contiene 26 variables y ofrece un equilibrio adecuado entre reducción de dimensionalidad, rendimiento predictivo, interpretabilidad y coherencia metodológica.

En la evaluación final sobre test, el mejor modelo dentro de esta representación fue:

```text
SVC lineal sobre OHE + MIFF-like
```

con resultados aproximados:

```text
ROC-AUC: 0.805
Recall clase positiva: 0.800
Balanced accuracy: 0.743
```

El mejor resultado global en test se obtuvo con:

```text
SVC lineal sobre Mixed + MIFF-like
```

con resultados aproximados:

```text
ROC-AUC: 0.8125
Recall clase positiva: 0.800
Balanced accuracy: 0.757
```

Aunque la Variante B obtiene el mejor resultado global en test, se mantiene `OHE + MIFF-like` como representación principal por coherencia con la metodología de referencia, menor dimensionalidad y mejor trazabilidad con la selección de variables realizada previamente.

## Interpretabilidad

El análisis de interpretabilidad muestra que las variables más relevantes se concentran en factores coherentes con el análisis de riesgo crediticio, como:

* finalidad del crédito;
* historial crediticio;
* estado de la cuenta corriente;
* nivel de ahorro;
* duración del crédito;
* importe solicitado;
* planes de financiación existentes.

El notebook también analiza los coeficientes de modelos lineales para estudiar qué categorías se asocian con mayor probabilidad de mal riesgo y cuáles se asocian con buen riesgo.

## Artefactos generados

El notebook exporta los artefactos necesarios para continuar con el siguiente experimento, especialmente el futuro notebook de autoencoder.

Entre los artefactos exportados se incluyen:

```text
data/processed/german_credit/exports/
data/processed/german_credit/autoencoder_input/
results/german_credit/feature_selection/tables/
models/german_credit/preprocessors/
```

Estos archivos incluyen:

* matrices procesadas;
* subconjuntos seleccionados;
* resultados de validación cruzada;
* resultados de test;
* listas de variables;
* preprocesadores;
* resumen experimental;
* manifest de exportación.

En la ejecución final del notebook se exportaron correctamente 32 artefactos, sin archivos faltantes.

## Estructura prevista del repositorio

La estructura general prevista del repositorio es:

```text
.
├── notebooks/
│   └── 02_german_credit_feature_selection.ipynb
├── data/
│   └── processed/
├── results/
│   └── german_credit/
├── models/
│   └── german_credit/
├── src/
├── README.md
└── requirements.txt
```

En esta fase, el elemento principal para revisión es el notebook `02_german_credit_feature_selection.ipynb`.

## Cómo ejecutar el notebook

El notebook está pensado para ejecutarse en Google Colab o en un entorno local con Jupyter.

Dependencias principales:

```text
numpy
pandas
matplotlib
seaborn
scikit-learn
ucimlrepo
joblib
```

Instalación orientativa:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn ucimlrepo joblib
```

Después, abrir y ejecutar:

```text
notebooks/02_german_credit_feature_selection.ipynb
```

El dataset se carga mediante `ucimlrepo`, por lo que no es necesario incluir manualmente el archivo original en el repositorio.

## Reproducibilidad

El experimento fija una semilla común:

```text
random_state = 42
```

La división train/test se realiza de forma estratificada y se reserva el conjunto de test hasta la evaluación final.

El notebook exporta un archivo de resumen de reproducibilidad con la configuración del experimento, dimensiones de las representaciones, resultados principales y rutas de exportación.

## Limitaciones

Los resultados deben interpretarse teniendo en cuenta varias limitaciones:

* German Credit es un dataset pequeño, con 1000 observaciones.
* Algunas variables pueden ser sensibles o problemáticas desde el punto de vista ético, como `personal_status_sex` o `foreign_worker`.
* La interpretación de coeficientes en variables codificadas mediante One-Hot Encoding debe hacerse con cautela.
* Los subconjuntos IVI-like, MIFF-like y RFF-like son una adaptación simplificada de la metodología de referencia.
* El objetivo principal no es maximizar el rendimiento, sino analizar el equilibrio entre rendimiento, reducción de dimensionalidad e interpretabilidad.

## Próximos pasos

El siguiente paso del proyecto será desarrollar el notebook:

```text
03_german_credit_autoencoder.ipynb
```

Este notebook utilizará como entrada principal la representación:

```text
OHE + MIFF-like
```

exportada desde el notebook actual, con el objetivo de estudiar el uso de un autoencoder como método complementario para analizar patrones de buen y mal riesgo crediticio.
