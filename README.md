# Forecasting de Demanda Multi-Producto para Optimización de Inventario en Retail del Sector Farmacéutico

## Descripción del proyecto
Sistema completo de previsión de demanda y optimización de inventario aplicado al retail farmacéutico, que combina:
* Ensemble heterogéneo de Deep Learning (Temporal Fusion Transformer, DeepAR, LightGBM y N-BEATS) para predecir la demanda de cada producto.
* Conformal Prediction para calibrar la incertidumbre de esas predicciones sin asumir distribuciones estadísticas rígidas (cobertura empírica: 86,36 %).
* Motor de decisión MILP (Programación Lineal Entera Mixta) que traduce las predicciones en una política de pedido óptima, ponderada según la criticidad clínica de cada categoría de producto (0,75 fármaco crítico / 0,175 alta rotación / 0,075 parafarmacia).

El sistema completo reduce el error de predicción un 18,0 % frente al baseline tradicional (ensemble) y hasta un 45,4 % con el mejor modelo individual (TFT), garantizando el 100 % de nivel de servicio en fármacos críticos.

---

## Notebooks incluidos
Este repositorio recoge el conjunto completo de notebooks del proyecto. En la memoria (Anexo I) solo se reproduce el output del notebook de evaluación principal (6 Folds); el resto se documenta aquí como respaldo íntegro del análisis de sensibilidad y las pruebas de robustez mencionadas en el cuerpo del trabajo.

| Notebook | Contenido | ¿En el Anexo I? |
| :--- | :--- | :---: |
| **PROYECTO_TFM_6_Folds.ipynb** | Evaluación definitiva del ensemble (TFT, DeepAR, LightGBM, N-BEATS), Conformal Prediction y motor MILP con validación de 6 folds. Es la fuente de todas las tablas y cifras principales de la memoria. | Sí |
| **PROYECTO_TFM_6_Folds_BASELINE_DEL_MÉTODO_TRADICIONAL_ (1).ipynb** | Cálculo del baseline tradicional (método estadístico clásico), usado como punto de comparación para medir la mejora de los modelos propuestos. | No |
| **PROYECTO_TFM_3_Folds.ipynb** | Análisis de sensibilidad del ensemble con validación de 3 folds, usado para comprobar la estabilidad del MAE. | No |
| **PROYECTO_TFM_1_Fold.ipynb** | Análisis de sensibilidad del ensemble con validación de 1 fold, complementario al anterior. | No |
| **PROYECTO_TFM_1_Fold_vN_Beats.ipynb** | Entrenamiento y evaluación específica del modelo N-BEATS. | No |
| **PROYECTO_TFM_1_Fold_(experimento_Pandemia)_FOLD_1.ipynb** | Prueba de estrés: simulación del pico pandémico de enero de 2021, con activación automática del techo de cobertura ampliado (180 %) en fármacos estacionales. | No |

*Nota: los notebooks 2 a 6 no se adjuntan en la memoria en el apartado “Anexos” por cuestiones de extensión, pero forman parte de la metodología descrita en el apartado de Validación de Robustez y Análisis de Sensibilidad. El código se conserva exactamente tal y como se ejecutó, sin modificaciones posteriores.*

---

## Resultados principales
Evaluación definitiva con 6 folds.

| Modelo | MAE | RMSE | WAPE (%) |
| :--- | :---: | :---: | :---: |
| **Baseline tradicional** | 55,63 | 98,46 | 229,56 |
| **LightGBM** | 39,18 | 74,73 | 159,38 |
| **TFT** | 30,39 | 113,92 | 100,00 |
| **DeepAR** | 53,89 | 110,68 | 178,01 |
| **N-BEATS** | 62,31 | 111,08 | 207,24 |
| **Ensemble** | 45,61 | 101,30 | 148,67 |

### Estructura del motor de decisión (MILP)
| Categoría | Peso en la función de coste | Techo/suelo de cobertura |
| :--- | :---: | :---: |
| **Fármaco Crítico** | 0,75 | 120 % |
| **Alta Rotación** | 0,175 | 120 % |
| **Parafarmacia** | 0,075 | 50 % |

---

## Fuente de datos
El proyecto emplea el Global Pharmacy Sales Dataset (2020-2025), publicado por Anne Mark en Kaggle: un conjunto de datos simulados de ventas diarias de farmacia a nivel global.

---

## Requisitos
Los notebooks están diseñados para ejecutarse en Google Colab. Las principales librerías utilizadas son:
* Modelos de forecasting: `pytorch_forecasting` (TFT, DeepAR), `darts` (N-BEATS), `lightgbm`, `xgboost`, `catboost`
* Optimización (motor MILP): `pulp`
* Deep Learning (base): `torch`, `pytorch_lightning` / `lightning.pytorch`
* Estadística y series temporales: `statsmodels`, `scipy.stats`
* Preprocesamiento y métricas: `scikit-learn`
* Manejo de datos: `pandas`, `numpy`
* Visualización: `matplotlib`, `seaborn`, `plotly`
* Otros: `holidays` (variables de calendario y festivos)

---

## Licencia
Este repositorio se comparte con fines académicos, como material de apoyo al Trabajo de Fin de Máster.