# Predicción del abandono de usuarios en aplicaciones móviles

## Anexo técnico del estudio

Este repositorio reúne el código, los procedimientos de análisis y las evidencias técnicas de un estudio académico sobre la predicción del abandono de usuarios (*churn*) en aplicaciones móviles mediante técnicas de aprendizaje automático.

Su finalidad es facilitar la revisión de la metodología utilizada, la construcción de las variables predictoras, la evaluación de los modelos y los resultados recogidos en la memoria académica.

## 1. Descripción del estudio

El análisis se desarrolla a partir de registros diarios de actividad correspondientes a 65.005 usuarios, distribuidos en doce perfiles de uso durante un periodo de 90 días.

A partir de esta información se construye una matriz de 176 variables predictoras que permiten estudiar distintas características del comportamiento, como la frecuencia, la intensidad, la recencia y la evolución temporal de la actividad.

Se comparan diferentes algoritmos de clasificación, entre ellos regresión logística, árboles de decisión, Random Forest y XGBoost.

El modelo finalmente seleccionado es una regresión logística con regularización L2, utilizando una ventana de observación de 60 días.

## 2. Organización del análisis

El procedimiento técnico se documenta mediante cinco notebooks de JupyterLab:

| Notebook | Contenido |
|---|---|
| `01_auditoria_dataset.ipynb` | Revisión de la estructura, integridad y calidad de los datos |
| `02_preparacion_dataset.ipynb` | Preparación de los registros y construcción de variables predictoras |
| `03_analisis_exploratorio.ipynb` | Estudio de distribuciones, patrones de actividad y correlaciones |
| `04_modelado.ipynb` | Entrenamiento, validación, comparación y selección de modelos |
| `05_sintesis_final.ipynb` | Síntesis de los resultados y conclusiones técnicas |

Los notebooks siguen el orden de desarrollo del estudio y permiten consultar el código utilizado y sus resultados guardados.

## 3. Resultados y evidencias

El proyecto incluye archivos de resultados, tablas, figuras, configuraciones y documentación de trazabilidad.

Estos materiales permiten relacionar los resultados presentados en la memoria con los procedimientos técnicos de los que proceden.

## 4. Herramientas utilizadas

El análisis se desarrolló en Python mediante JupyterLab.

Se utilizaron principalmente Pandas, NumPy, scikit-learn, XGBoost, Matplotlib y Joblib.

## 5. Alcance y limitaciones

La variable objetivo `churn` está definida en el conjunto de datos original, pero no se dispone de información suficiente sobre su construcción temporal.

Por este motivo, los resultados permiten evaluar la capacidad de los modelos para distinguir entre usuarios clasificados como *churn* y *no churn*, pero no demuestran que el abandono pueda anticiparse con una antelación determinada.

La evaluación también debe interpretarse teniendo en cuenta las limitaciones documentadas sobre el uso histórico del conjunto de prueba y la ausencia de una validación externa prospectiva independiente.

## 6. Reproducibilidad

Los notebooks y los archivos de resultados documentan las operaciones realizadas durante el desarrollo del estudio.

La reproducción completa depende de la disponibilidad del conjunto de datos original y de las dependencias del entorno de ejecución.

## 7. Finalidad académica

Este repositorio constituye un anexo técnico digital de la memoria académica. Su propósito es facilitar la consulta, comprobación y trazabilidad del análisis realizado.
