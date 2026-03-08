# 📉 Telecom X - Parte 2: Predicción de Churn de Clientes

## 🎯 Propósito del Proyecto
Este proyecto, desarrollado como parte del curso de Data Science e IA de Alura ONE, tiene como objetivo principal construir un modelo de Machine Learning capaz de predecir el **Churn (cancelación o fuga)** de los clientes de la empresa de telecomunicaciones Telecom X. Al identificar anticipadamente qué clientes tienen una alta probabilidad de abandonar el servicio, la empresa puede tomar decisiones proactivas y diseñar campañas de retención, minimizando así la pérdida de ingresos.

## 📂 Estructura del Proyecto
El repositorio está organizado de la siguiente manera:
* `TelecomX_parte2_proyecto_Alura_ONE_G9.ipynb`: Cuaderno principal (Jupyter/Colab) que contiene todo el código, desde la ingesta de datos hasta la evaluación de los modelos.
* `Telecom_churn.json`: Archivo de datos original proporcionado para el análisis (cargado vía URL en el cuaderno).
* `README.md`: Documentación técnica y descriptiva del proyecto.

## 🛠️ Proceso de Preparación de los Datos
Para garantizar la calidad de los datos antes del modelado, se realizó un riguroso proceso de preprocesamiento:
* **Extracción y Aplanamiento:** Los datos se obtuvieron mediante peticiones web (`requests`) y se aplanaron desde un formato JSON anidado utilizando `pd.json_normalize()`.
* **Tratamiento de Valores Ocultos:** Se detectaron y eliminaron registros con strings vacíos (`' '`) en variables clave, los cuales no eran detectables por los métodos tradicionales de nulos.
* **Clasificación y Codificación:** Se clasificaron las variables en numéricas y categóricas. Las variables sin poder predictivo (`customerID`, `gender`) fueron eliminadas. Las categóricas fueron transformadas a formato numérico mediante One-Hot Encoding (`pd.get_dummies()`).
* **Separación de Datos:** Los datos se dividieron en conjuntos de **Entrenamiento (70%)** y **Prueba (30%)** utilizando `train_test_split` con el parámetro `stratify` para mantener la proporción de la clase objetivo.
* **Normalización:** Se aplicó `StandardScaler` a las variables numéricas (después de la separación de datos para evitar Data Leakage) para homogenizar las escalas de medición.

## 🧠 Justificaciones de Modelización
Durante la fase de modelado se implementó una evolución iterativa:
* **Problema de Negocio (Desbalanceo y Recall):** El dataset presentaba un fuerte desbalanceo (73% No Churn vs 26% Churn). Dado que en este modelo de negocio el costo de un Falso Negativo (no detectar a un cliente que se va) es mucho mayor que el de un Falso Positivo (ofrecer un descuento a un cliente que se queda), **se priorizó la métrica de Recall**.
* **Modelos Base:** Se probaron inicialmente Regresión Logística y Random Forest. La Regresión Logística balanceada obtuvo un desempeño inicial aceptable (Recall del 79%), pero se buscó optimizar el resultado.
* **Modelos Avanzados y GridSearchCV:** Se implementó Validación Cruzada (`GridSearchCV`) optimizando hiperparámetros para modelos robustos como **XGBoost** y **LightGBM**. 
* **El Modelo Ganador:** **LightGBM** fue seleccionado como el modelo final. Logró un excelente equilibrio para el negocio: un **Recall del 81%** (atrapando a 8 de cada 10 desertores) manteniendo los Falsos Positivos en un margen comercialmente aceptable, superando la tendencia al sobreajuste detectada en XGBoost.

## 📊 Insights del Análisis Exploratorio (EDA)
Durante la fase exploratoria se descubrieron patrones clave de comportamiento:
* **El factor "Tenure":** A mayor antigüedad del cliente, la probabilidad de fuga disminuye drásticamente.
* **El factor "Contrato":** Existe una fuerte correlación positiva entre las cancelaciones y los clientes que poseen contratos de renovación mensual (`Month-to-month`). Estos clientes son los más volátiles y el principal foco de alerta.
* **El factor edad**: Los clientes senior tiene el doble de probabilidad de hacer churn.
* **El factor soltero**: Clientes sin pareja son mas propicios a hacer churn.

## 🚀 Instrucciones de Ejecución
Para reproducir este proyecto en un entorno local o en Google Colab:
1. Asegúrate de tener instaladas las bibliotecas ejecutando: `pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm requests`.
2. Clona este repositorio y abre el archivo `TelecomX_parte2_proyecto_Alura_ONE_G9.ipynb`.
3. Ejecuta las celdas en orden secuencial. El cuaderno está configurado para descargar automáticamente los datos crudos desde la web mediante la librería `requests`, por lo que no es necesario gestionar archivos locales manualmente.
