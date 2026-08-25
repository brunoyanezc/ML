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
Se aplicó **Winsorización** (clip al 1% y 99%) en lugar de eliminación por IQR. Esto permitió suavizar los extremos (especialmente en WindSpeed con 71 casos) sin perder registros, manteniendo el dataset original intacto.

## Selección de Características
A pesar de que el EDA lineal arroja que solo 3 variables correlacionan fuertemente con el target, se conservan las 7 variables predictoras para evitar un sesgo lineal prematuro antes del modelado de la Unidad 2.

## División de Datos (Split)
Se seleccionó el método **Hold-Out (70/15/15)** por sobre *K-Fold* y *Train-Test (80/20)* por generar conjuntos independientes fijos para validación y pruebas.
- **Entrenamiento (70%):** 924 registros con 7 variables.
- **Validación (15%):** 198 registros con 7 variables.
- **Prueba (15%):** 198 registros con 7 variables.

## Casos de Uso
Regresión ambiental, predicción de contaminación y salud pública.

## Notas
- Todos los datos están en formato CSV
- Las variables están en inglés
- El entorno requiere Python 3.11 y las librerías: `pandas`, `numpy`, `matplotlib`, `seaborn` y `scikit-learn` (especificadas en el archivo `requirements.txt`).
- No requiere autenticación para acceder
- Repositorio estructurado para asegurar la reproducibilidad analítica exigida.
