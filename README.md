# Predicción del abandono de usuarios (churn) en aplicaciones móviles

Proyecto desarrollado en el marco del **Máster Universitario en Inteligencia Artificial Aplicada a Entornos Empresariales y Financieros**, de la Universidad Internacional de Andalucía (UNIA).

## 1. Presentación del proyecto

Este trabajo analiza la posibilidad de identificar patrones de abandono (*churn*) en usuarios de aplicaciones móviles a partir de sus registros de actividad.

El estudio parte de un conjunto de datos que recoge el comportamiento diario de 65.005 usuarios durante un periodo máximo de 90 días. A partir de esta información se construyen variables que permiten estudiar la frecuencia de uso, la intensidad de la actividad, la recencia y su evolución a lo largo del tiempo.

El objetivo es comparar diferentes modelos de clasificación y determinar hasta qué punto la cantidad de historial disponible influye en su rendimiento. También se consideran aspectos como la interpretación de los resultados, la complejidad de los modelos y su posible utilidad para la gestión de la retención de usuarios.

El repositorio contiene el código utilizado durante el estudio, la documentación del entorno de trabajo y las principales evidencias gráficas de los resultados.

## 2. Datos utilizados

El análisis se realizó sobre un conjunto de datos con las siguientes características:

- **Usuarios totales:** 65.005.
- **Conjunto de entrenamiento:** 52.004 usuarios.
- **Conjunto de prueba:** 13.001 usuarios.
- **Proporción de usuarios etiquetados como churn:** aproximadamente el 23 %.
- **Periodo máximo de actividad registrado:** 90 días.
- **Perfiles de actividad:** 12 secuencias diarias por usuario.
- **Variables predictoras finales:** 176.
- **Ventanas de observación estudiadas:** 30, 60 y 90 días.

Los registros originales se transformaron en una matriz de características, con una fila por usuario y variables que resumen distintos aspectos de su comportamiento.

Esta transformación permite utilizar modelos de clasificación convencionales sin perder por completo la información sobre cómo evoluciona la actividad durante el periodo observado.

### Limitación temporal de los datos

La documentación disponible no permite establecer con certeza cuándo se definió la etiqueta de abandono ni su relación temporal exacta con los registros de actividad.

Por esta razón, las ventanas de 30, 60 y 90 días representan distintas cantidades de historial disponible, pero no permiten determinar con cuánta antelación podría predecirse el abandono.

Los resultados deben interpretarse como la capacidad de los modelos para clasificar a los usuarios según la etiqueta proporcionada por el conjunto de datos, sin atribuirles una capacidad de predicción temprana que no ha podido comprobarse.

## 3. Metodología

El estudio se desarrolló siguiendo un proceso de preparación de datos, análisis exploratorio, construcción de variables, entrenamiento y evaluación de modelos.

### Preparación de datos y construcción de variables

A partir de las secuencias de actividad diaria se elaboraron indicadores para representar distintos aspectos del comportamiento de los usuarios:

- **Frecuencia:** regularidad con la que se registra actividad.
- **Intensidad:** volumen de actividad acumulada durante el periodo observado.
- **Recencia:** tiempo transcurrido desde la última actividad registrada.
- **Evolución temporal:** cambios en el comportamiento a lo largo del periodo.
- **Tendencia:** dirección e intensidad de las variaciones de actividad.

El conjunto definitivo quedó formado por 176 variables predictoras por usuario.

Para comparar la influencia de la cantidad de información disponible, se construyeron representaciones correspondientes a los primeros 30, 60 y 90 días de actividad.

### Modelos evaluados

Se estudiaron diferentes algoritmos de clasificación supervisada:

1. Clasificador de referencia (*Dummy Classifier*).
2. Regresión logística con regularización L2.
3. Regresión logística con ponderación equilibrada de clases.
4. Árbol de decisión.
5. Random Forest.
6. XGBoost.

La comparación permitió analizar alternativas con distintos niveles de complejidad e interpretabilidad.

### Validación y evaluación

Durante el desarrollo se utilizaron particiones diferenciadas de entrenamiento y prueba, junto con validación cruzada estratificada de cinco pliegues.

Para evaluar los modelos se consideraron distintas métricas, ya que el porcentaje de usuarios clasificados como churn es inferior al de aquellos que permanecen activos.

Las principales métricas utilizadas fueron:

- **Precision:** proporción de predicciones positivas que corresponden realmente a usuarios churn.
- **Recall:** proporción de usuarios churn identificados correctamente.
- **F1 y F2:** medidas que combinan precisión y recall, dando F2 mayor importancia a la detección de la clase positiva.
- **AUC-ROC:** capacidad del modelo para distinguir entre las dos clases.
- **Average Precision (AP):** resumen del comportamiento de la curva precisión-recall.
- **Brier score y log loss:** medidas de calidad de las probabilidades estimadas.

También se examinaron la matriz de confusión, la calibración de las probabilidades y el efecto del umbral utilizado para clasificar a los usuarios.

## 4. Modelo seleccionado y resultados

El modelo finalmente seleccionado fue una **regresión logística con regularización L2**, precedida de una etapa de estandarización de las variables.

Sus principales características son:

- **Historial utilizado:** 60 días.
- **Número de predictores:** 176.
- **Umbral de clasificación:** 0,20.
- **Algoritmo:** regresión logística L2.

La elección se basó en el equilibrio entre rendimiento, sencillez e información necesaria. Aunque algunas alternativas alcanzaron resultados ligeramente superiores, la regresión logística con 60 días de historial ofreció un rendimiento comparable utilizando menos información temporal.

### Resultados en el conjunto de prueba

La evaluación documentada del modelo seleccionado, sobre 13.001 usuarios, produjo los siguientes resultados:

| Métrica | Resultado |
|---|---:|
| Average Precision (AP) | 0,998693 |
| AUC-ROC | 0,999578 |
| Precision | 0,970472 |
| Recall | 0,993284 |
| F1 | 0,981746 |
| F2 | 0,988636 |
| Brier score | 0,005045 |
| Log loss | 0,019732 |

La matriz de confusión registró:

| Resultado | Usuarios |
|---|---:|
| Verdaderos negativos | 9.933 |
| Falsos positivos | 90 |
| Falsos negativos | 20 |
| Verdaderos positivos | 2.958 |

Estos valores reflejan una elevada capacidad de clasificación sobre el conjunto de datos estudiado. Sin embargo, no permiten asegurar que el modelo mantenga el mismo comportamiento en otras aplicaciones o con nuevos usuarios.

Debe tenerse en cuenta, además, que el conjunto de prueba fue consultado durante fases anteriores del desarrollo. Por tanto, sus resultados no deben presentarse como una evaluación completamente independiente de todas las decisiones tomadas durante el estudio.

## 5. Evidencias gráficas

El repositorio incluye siete figuras que recogen los principales resultados de la evaluación y facilitan su interpretación.

### Evaluación del modelo final

- [Matriz de confusión](evidencias/figura_matriz_confusion_test_60d.png): distribución de los aciertos y errores de clasificación.
- [Curva ROC](evidencias/figura_curva_roc_test_60d.png): capacidad del modelo para distinguir entre usuarios churn y no churn.
- [Curva precisión-recall](evidencias/figura_curva_precision_recall_test_60d.png): comportamiento de la precisión y el recall para distintos umbrales.
- [Curva de calibración](evidencias/figura_calibracion_test_60d.png): relación entre las probabilidades estimadas y la frecuencia observada de abandono.
- [Captura acumulada de churn](evidencias/figura_captura_acumulada_test_60d.png): concentración de los usuarios churn al ordenarlos según el riesgo estimado.

### Comparación e interpretación

- [Comparación entre modelos y ventanas de observación](evidencias/figura_comparacion_ventanas_modelos.png): resultados de las alternativas evaluadas con 30, 60 y 90 días de historial.
- [Coeficientes principales del modelo](evidencias/figura_coeficientes_principales_60d.png): variables con mayor peso en la regresión logística seleccionada.

Las figuras corresponden a los resultados obtenidos durante el desarrollo original. No proceden de una nueva ejecución realizada para preparar este repositorio.

Los coeficientes permiten interpretar asociaciones entre las variables y la clasificación estimada, pero no deben considerarse pruebas de relaciones causales.

### Resultados numéricos agregados

Además de las figuras, se incluyen tres archivos CSV con los principales resultados de evaluación y comparación de modelos:

- [Métricas del modelo final](resultados_agregados/modelado_metricas_test_externo_60d.csv): resultados de la regresión logística L2 con 60 días de historial sobre el conjunto de prueba.
- [Comparación de modelos mediante validación cruzada](resultados_agregados/modelado_resumen_cv_modelos.csv): resumen de las métricas obtenidas por los seis modelos evaluados.
- [Comparación de ventanas de observación](resultados_agregados/modelado_comparacion_temporal_oof.csv): resultados de la comparación de modelos con 30, 60 y 90 días de historial.

Estos archivos permiten consultar las métricas utilizadas para comparar las alternativas y justificar la selección del modelo final. No incluyen registros ni predicciones individuales de usuarios.

## 6. Organización del proyecto

El estudio se desarrolló inicialmente en cinco notebooks, organizados según las distintas etapas del análisis.

| Notebook | Contenido |
|---|---|
| [01_auditoria_dataset.ipynb](notebooks/01_auditoria_dataset.ipynb) | Revisión inicial del conjunto de datos, estructura y comprobaciones de calidad. |
| [02_preparacion_dataset.ipynb](notebooks/02_preparacion_dataset.ipynb) | Preparación de los registros y construcción de las variables predictoras. |
| [03_analisis_exploratorio.ipynb](notebooks/03_analisis_exploratorio.ipynb) | Análisis descriptivo y exploración de los patrones de actividad. |
| [04_modelado.ipynb](notebooks/04_modelado.ipynb) | Entrenamiento, comparación y evaluación de modelos, incluida la reconstrucción de las ventanas de 30 y 60 días. |
| [05_sintesis_final.ipynb](notebooks/05_sintesis_final.ipynb) | Recopilación e interpretación de los resultados finales. |

Posteriormente se incorporó un sexto notebook:

**[06_verificacion_independiente.ipynb](notebooks/06_verificacion_independiente.ipynb)**

Este cuaderno realiza comprobaciones complementarias sobre la consistencia de los resultados y las predicciones de los modelos evaluados con 60 días de historial. Se elaboró después de finalizar el desarrollo principal y no intervino en la selección original del modelo.

## 7. Entorno de trabajo

El proyecto se desarrolló en Windows, utilizando Python, JupyterLab y un entorno de Conda denominado `churn_tfm`.

Las principales herramientas empleadas fueron:

- Python.
- Pandas y NumPy.
- Scikit-learn.
- XGBoost.
- Matplotlib.
- Joblib.
- JupyterLab.

El archivo [environment.yml](environment.yml) recoge las versiones instaladas en el entorno al preparar el repositorio. Estas pueden diferir de las utilizadas durante algunas fases anteriores del desarrollo.

### Instalación del entorno

Para preparar el entorno en Windows es necesario disponer previamente de Conda o Miniconda.

Desde una terminal situada en la carpeta principal del repositorio, se pueden ejecutar los siguientes comandos:

```bash
conda env create -f environment.yml
conda activate churn_tfm
jupyter lab
```

El archivo de configuración facilita la instalación de las dependencias, aunque no garantiza por sí solo que el estudio pueda reproducirse íntegramente en cualquier equipo.

## 8. Datos y ejecución de los notebooks

El repositorio reúne el código desarrollado durante el estudio, los resultados documentados y sus principales evidencias gráficas.

No incluye los datos originales, los archivos intermedios de procesamiento, las predicciones individuales ni los modelos guardados. Por este motivo, aunque permite examinar la metodología y los procedimientos utilizados, la reproducción completa del análisis requiere disponer de los datos de origen y generar los archivos necesarios durante la ejecución de los notebooks.

### Estructura de carpetas

Para ejecutar el proyecto con los datos originales, se necesita una organización de archivos como la siguiente:

```text
mobile-app-churn-prediction/
│
├── notebooks/
│   ├── 01_auditoria_dataset.ipynb
│   ├── 02_preparacion_dataset.ipynb
│   ├── 03_analisis_exploratorio.ipynb
│   ├── 04_modelado.ipynb
│   ├── 05_sintesis_final.ipynb
│   └── 06_verificacion_independiente.ipynb
│
├── evidencias/
│   └── (7 figuras del estudio)
│
├── datos/
│   └── Dataset/
│       ├── main.csv
│       ├── train/
│       │   └── train_cxid.csv
│       ├── test/
│       │   └── test_cxid.csv
│       ├── usage_data/
│       │   └── (12 archivos CSV de actividad)
│       └── raw_dump/
│           └── full-anonymized-data.txt
│
├── resultados/
├── modelos/
│
├── README.md
├── environment.yml
└── .gitignore
```

Las carpetas `datos`, `resultados` y `modelos` están excluidas de GitHub mediante `.gitignore`. La carpeta `evidencias`, en cambio, contiene las siete figuras que acompañan a la documentación del estudio.

### Orden de ejecución

Los cinco primeros notebooks deben ejecutarse en el siguiente orden:

1. `01_auditoria_dataset.ipynb`
2. `02_preparacion_dataset.ipynb`
3. `03_analisis_exploratorio.ipynb`
4. `04_modelado.ipynb`
5. `05_sintesis_final.ipynb`

Para ello, se debe iniciar JupyterLab desde la carpeta principal del proyecto y abrir los cuadernos dentro de `notebooks`. Algunas instrucciones utilizan rutas relativas al directorio de trabajo, por lo que deben ejecutarse desde esa ubicación.

Antes de comenzar, es necesario disponer de los archivos de datos en las rutas correspondientes y crear las carpetas destinadas a los resultados y modelos. En particular, el notebook 02 necesita que exista la carpeta `resultados`.

El notebook 06 se ejecuta por separado, una vez disponibles los archivos generados durante las etapas anteriores. Su funcionamiento también requiere determinados resultados intermedios y el modelo final guardado.

Los cuadernos publicados no incluyen salidas de ejecución. Esta decisión permite compartir el código sin incorporar accidentalmente datos individuales o resultados detallados por usuario.

La reproducción completa del estudio a partir de esta versión del repositorio no ha sido comprobada.

## 9. Disponibilidad de los datos y limitaciones

Los datos originales no se distribuyen con el repositorio. Antes de compartirlos sería necesario verificar las condiciones de uso y los permisos correspondientes.

También se han excluido las predicciones individuales, los modelos serializados y los archivos intermedios generados durante el procesamiento y entrenamiento.

Estas restricciones no impiden consultar el código, conocer las decisiones metodológicas o examinar los resultados y figuras publicados. Sin embargo, limitan la posibilidad de repetir el análisis completo sin disponer previamente de los archivos originales.

El estudio presenta, además, algunas limitaciones que deben tenerse en cuenta al interpretar los resultados:

- La definición temporal de la etiqueta churn no está suficientemente documentada.
- No puede demostrarse con qué antelación se identifica el abandono.
- El conjunto de prueba fue consultado durante el desarrollo, por lo que su evaluación debe interpretarse teniendo en cuenta esta circunstancia.
- Los resultados corresponden al conjunto de datos analizado y no garantizan el mismo rendimiento en otras aplicaciones.
- Las relaciones observadas entre las variables de actividad y el abandono no implican causalidad.

## 10. Consideraciones finales

El estudio muestra que los registros de actividad pueden aportar información útil para distinguir entre usuarios con diferentes patrones de permanencia y abandono.

Los resultados también ponen de manifiesto que un modelo relativamente sencillo, como la regresión logística con regularización L2, puede alcanzar un rendimiento comparable al de alternativas más complejas cuando se dispone de variables adecuadas.

No obstante, su posible aplicación a situaciones reales requeriría comprobar cómo se define temporalmente el abandono, evaluar el comportamiento del modelo con nuevos usuarios y garantizar un tratamiento adecuado de los datos.

Este repositorio permite consultar el desarrollo técnico del trabajo y sus principales resultados, dejando constancia tanto de las decisiones adoptadas como de las limitaciones que condicionan su interpretación.

