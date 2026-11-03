# Predicción del abandono de usuarios (churn) en aplicaciones móviles

Proyecto académico desarrollado en el marco del **Máster Universitario en Inteligencia Artificial Aplicada a Entornos Empresariales y Financieros** de la Universidad Internacional de Andalucía (UNIA).

## Objetivo

Analizar si la actividad registrada durante distintos periodos de observación permite clasificar a los usuarios según la etiqueta de abandono (*churn*) proporcionada por el conjunto de datos. Se comparan diferentes algoritmos y cantidades de historial disponible, prestando atención tanto a la calidad de la clasificación como a la interpretación de los resultados.

## Datos y alcance

- **Usuarios:** 65.005.
- **Conjunto de entrenamiento:** 52.004 usuarios.
- **Conjunto de prueba:** 13.001 usuarios.
- **Prevalencia aproximada de churn:** 23 %.
- **Actividad:** registros diarios organizados en 12 secuencias de uso por usuario, con un máximo de 90 días de historial.
- **Variables predictoras finales:** 176 por usuario, construidas a partir de indicadores de actividad, recencia, frecuencia, intensidad y evolución temporal.
- **Ventanas de observación comparadas:** 30, 60 y 90 días.

**Limitación temporal importante:** la documentación disponible no permite determinar con certeza la fecha de definición de la etiqueta *churn* ni su relación cronológica exacta con los registros de actividad. Por ello, las ventanas de 30, 60 y 90 días indican **cantidad de historial utilizada**, no horizontes de predicción anticipada demostrados. Los resultados deben interpretarse como capacidad para clasificar la etiqueta facilitada, sin afirmar una anticipación temporal del abandono.

## Metodología

Se emplean técnicas de preparación de datos, ingeniería de características y evaluación supervisada. El análisis contempla un clasificador de referencia, regresión logística con regularización L2, regresión logística con pesos de clase equilibrados, árbol de decisión, *Random Forest* y XGBoost.

Durante el desarrollo se utilizaron particiones de entrenamiento y prueba y validación cruzada estratificada. Se examinaron métricas adecuadas para una clasificación con clases de distinto tamaño, entre ellas precisión, exhaustividad (*recall*), F1, F2, AUC-ROC, área bajo la curva precisión-recall (*average precision*), Brier score y *log loss*.

La comparación de ventanas de observación permitió seleccionar una solución de menor complejidad que otras alternativas evaluadas, de acuerdo con los criterios de rendimiento establecidos en el estudio.

## Modelo seleccionado y resultados documentados

El modelo final documentado es una **regresión logística L2**, precedida de estandarización de variables, que utiliza **60 días de historial**, **176 predictores** y un **umbral de decisión de 0,20**.

Resultados registrados para el conjunto de prueba de 13.001 usuarios:

| Métrica | Valor |
|---|---:|
| Average precision (AP) | 0,998693 |
| AUC-ROC | 0,999578 |
| Precisión | 0,970472 |
| Recall | 0,993284 |
| F1 | 0,981746 |
| F2 | 0,988636 |
| Brier score | 0,005045 |
| Log loss | 0,019732 |

Matriz de confusión registrada: **VN = 9.933, FP = 90, FN = 20, VP = 2.958**.

**Interpretación y cautelas:** estas métricas corresponden al protocolo y los datos utilizados en el proyecto. No constituyen por sí mismas evidencia de funcionamiento en producción, generalización a otras aplicaciones o predicción temprana del abandono. El conjunto de prueba fue consultado durante fases históricas del trabajo, por lo que no debe presentarse como una evaluación externa completamente independiente e intocada durante todo el desarrollo.

## Organización de los cuadernos

Los cinco primeros notebooks forman el flujo de trabajo principal, en este orden:

1. `notebooks/01_auditoria_dataset.ipynb`: auditoría inicial del conjunto de datos.
2. `notebooks/02_preparacion_dataset.ipynb`: preparación de datos y construcción de variables.
3. `notebooks/03_analisis_exploratorio.ipynb`: análisis exploratorio.
4. `notebooks/04_modelado.ipynb`: desarrollo, comparación y evaluación de modelos; incluye la reconstrucción de ventanas de 30 y 60 días.
5. `notebooks/05_sintesis_final.ipynb`: síntesis de resultados.

El notebook `notebooks/06_verificacion_independiente.ipynb` se incorporó después como **comprobación complementaria**. No forma parte del proceso original de desarrollo y selección del modelo.

## Entorno de ejecución

El proyecto se desarrolló utilizando Python y JupyterLab en Windows, dentro del entorno Conda `churn_tfm`. Las versiones **presentes en el entorno al preparar esta publicación** están registradas en [`environment.yml`](environment.yml). No se ha acreditado que todas coincidan con las que había instaladas en cada fase histórica del desarrollo.

Para crear un entorno a partir del archivo **en Windows** (con Conda o Miniconda instalado), abre una terminal en la carpeta raíz del repositorio y ejecuta:

```bash
conda env create -f environment.yml
conda activate churn_tfm
jupyter lab
```

### Qué se necesita para ejecutar los notebooks

Este repositorio es una **publicación del código y de los resultados agregados documentados, no un paquete de reproducción autónoma**. Los archivos de datos originales, los artefactos de entrenamiento, las predicciones por cliente y los modelos serializados no se incluyen en la versión pública.

El código espera esta estructura, si se dispone legítimamente de los datos de origen:

```text
Proyecto_Churn_GitHub/
├── notebooks/              # Seis notebooks publicados
├── datos/
│   └── Dataset/
│       ├── main.csv
│       ├── train/train_cxid.csv
│       ├── test/test_cxid.csv
│       ├── usage_data/     # Los 12 CSV de perfiles de uso
│       └── raw_dump/full-anonymized-data.txt
├── resultados/             # Archivos intermedios generados; no publicados
├── modelos/                # Modelos serializados; no publicados
└── evidencias/             # Figuras y evidencias generadas; no publicadas
```

Las carpetas `datos`, `resultados`, `modelos` y `evidencias` están excluidas mediante `.gitignore`, por lo que **no aparecerán tras clonar el repositorio**. Cuando se disponga de los datos autorizados, hay que crear esas carpetas antes de ejecutar el flujo; el notebook 02 necesita que `resultados` exista para guardar sus primeros archivos.

Para ejecutar desde JupyterLab, inicia `jupyter lab` en la raíz del repositorio, abre los cuadernos desde la carpeta `notebooks/` y ejecútalos allí, en orden: `01` → `02` → `03` → `04` → `05`. Los notebooks 01 y 02 calculan la raíz del proyecto a partir del directorio de trabajo de Jupyter, que debe ser `notebooks/`. El notebook 06 es una auditoría posterior y **requiere además los resultados, checkpoints y el modelo final generados previamente**.

En la copia publicable de `04_modelado.ipynb` se ha convertido en una nota la celda que ejecutaba `%pip install xgboost-cpu`: esa dependencia ya está incluida en `environment.yml`. Es un ajuste de instalación; **no se han cambiado los algoritmos, parámetros, datos ni resultados del análisis**.

Los notebooks se publican **sin salidas de ejecución** para evitar difundir datos individuales. Las conclusiones escritas en celdas Markdown reflejan los resultados del estudio original y no una nueva ejecución del código sobre los archivos aquí publicados. No se ha comprobado la reproducción íntegra del estudio a partir de esta copia pública.

## Disponibilidad de datos y archivos

Los archivos originales de datos no se publican automáticamente en este repositorio: primero debe comprobarse que su licencia o autorización permite la redistribución. Tampoco deben publicarse identificadores de usuarios, predicciones individuales, credenciales, rutas personales, archivos intermedios pesados ni información confidencial.

La publicación de los notebooks, figuras, resultados agregados y modelos guardados está sujeta a una revisión previa para descartar información individual o sensible y para verificar los permisos aplicables. La ausencia de datos originales puede limitar la reproducción completa del procesamiento y del entrenamiento.

## Uso académico

Este repositorio documenta un estudio de clasificación de *churn* basado en un conjunto de datos específico. Las asociaciones identificadas entre actividad y etiqueta no deben interpretarse como relaciones causales. Antes de aplicar cualquier resultado a decisiones reales de retención sería necesario validar las definiciones temporales, comprobar la generalización y revisar las implicaciones de privacidad y uso legítimo de los datos.

