# ML (eINGT1029-1) Calidad del aire PM2.5

## Descripción
Framework CRISP-DM para el desarrollo de un pipeline reproducible de Machine Learning enfocado en la estimación de material particulado (PM2.5) utilizando variables meteorológicas y contaminantes atmosféricos.

## Target
PM2_5 (μg/m³)

## Tamaño
1,320 registros, 8 variables

## Diccionario de Variables

| Variable | Tipo | Descripción |
|----------|------|-------------|
| SO2 | float | Dióxido de azufre (μg/m³) |
| NO2 | float | Dióxido de nitrógeno (μg/m³) |
| O3 | float | Ozono (μg/m³) |
| Temperature | float | Temperatura (°C) |
| Humidity | float | Humedad relativa (%) |
| Pressure | float | Presión atmosférica (hPa) |
| WindSpeed | float | Velocidad del viento (m/s) |
| PM2_5 | float | Target: Partículas PM2.5 (μg/m³) |

## Valores Faltantes y Duplicados
Ninguno. Inspección realizada mediante `df.isnull().sum()` y `df.duplicated().sum()`.

## Outliers y Limpieza
Se aplicó Winsorización (clip al 1% y 99%) en lugar de eliminación por IQR. Esto permitió suavizar los extremos (especialmente en WindSpeed con 71 casos) sin perder registros, manteniendo el dataset original intacto.

## Selección de Características
A pesar de que el EDA lineal arroja que solo 3 variables correlacionan fuertemente con el target, se conservan las 7 variables predictoras para evitar un sesgo lineal prematuro antes del modelado de la Unidad 2.

## División de Datos (Split)
Se seleccionó el método **Hold-Out (70/15/15)** por sobre *K-Fold* y *Train-Test (80/20)* por generar conjuntos independientes fijos para validación y pruebas.
- **Entrenamiento (70%):** 924 registros con 7 variables.
- **Validación (15%):** 198 registros con 7 variables.
- **Prueba (15%):** 198 registros con 7 variables.

## Unidad 2: Modelado Supervisado y Evaluación

A partir del dataset preprocesado en la Unidad 1 se entrenaron y compararon distintos algoritmos de Machine Learning para la predicción de PM2.5.

### Modelos Evaluados

- Baseline (Dummy Regressor)
- Elastic Net
- Random Forest Regressor
- XGBoost Regressor
- Red Neuronal (MLP)

### Métricas Utilizadas

- RMSE
- MAE
- R²

### Optimización de Hiperparámetros

Se utilizó GridSearchCV para optimizar:

- Elastic Net
- Random Forest
- XGBoost

mediante validación cruzada de 5 particiones.

### Resultado Final

Tras la comparación de modelos y la optimización de hiperparámetros, XGBoost fue seleccionado como modelo final.

Resultados obtenidos sobre el conjunto Test:

- RMSE: 9.747
- MAE: 7.827
- R²: 0.764

### Interpretación

Los resultados indican que las variables meteorológicas y los contaminantes atmosféricos contienen información suficiente para explicar aproximadamente un 76% de la variabilidad observada en PM2.5, permitiendo construir modelos predictivos útiles para monitoreo ambiental y apoyo a la toma de decisiones.


## Unidad 3: Aprendizaje No Supervisado y Clustering

## Preparación de Datos

Para clustering se mantuvo la estrategia de Winsorización definida en la Unidad 1.

No fue necesario aplicar One-Hot Encoding debido a que todas las variables predictoras correspondían a variables numéricas continuas.

Se aplicó normalización mediante StandardScaler para evitar que variables medidas en distintas escalas dominaran artificialmente la formación de clusters.

## PCA

Se evaluó la reducción de dimensionalidad mediante PCA utilizando:

- Scree Plot
- Varianza explicada acumulada

Se comparó el desempeño de los algoritmos con y sin PCA.

Los resultados mostraron que PCA no modificó significativamente la calidad de los agrupamientos obtenidos, sugiriendo que la estructura original de los datos ya era adecuadamente representativa.

## Benchmark de Algoritmos

Se compararon algoritmos basados en distancia y densidad:

- KMeans
- Agglomerative Clustering
- Birch
- DBSCAN
- OPTICS

### Resultados

| Modelo | Silhouette |
|----------|----------:|
| KMeans | **0.113** |
| Birch | 0.066 |
| Agglomerative | 0.050 |
| DBSCAN | -0.249 |
| OPTICS | -0.315 |

## Modelo Seleccionado

**KMeans** fue seleccionado por presentar el mejor Silhouette Score y el mejor desempeño global entre los algoritmos evaluados.

## Interpretación mediante PM2.5

Aunque PM2.5 no fue utilizado durante el entrenamiento, los clusters identificados mostraron diferencias claras en los niveles de contaminación:

| Cluster | Interpretación | PM2.5 Promedio (μg/m³) |
|----------|----------|----------:|
| 0 | Baja contaminación | 21.59 |
| 2 | Contaminación intermedia | 44.01 |
| 1 | Alta contaminación | 58.06 |

Los clusters obtenidos permitieron distinguir observaciones asociadas a distintos niveles de contaminación atmosférica utilizando únicamente variables meteorológicas y contaminantes ambientales.


## Casos de Uso
Regresión ambiental, predicción de contaminación y salud pública.

## Notas
- Todos los datos están en formato CSV
- Las variables están en inglés
- El entorno requiere Python 3.11 y las librerías: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn` y `xgboost`  (especificadas en el archivo `requirements.txt`).
- No requiere autenticación para acceder
- Repositorio estructurado para asegurar la reproducibilidad analítica exigida.
