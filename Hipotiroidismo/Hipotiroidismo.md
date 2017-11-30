# Hipotiroidismo

# Descripción del problema

La tiroides es una glándula que se encuentra ubicada en el cuello, encima de la tráquea y que cumple una función muy importante en el cuerpo, ya que es la responsable de regular el metabolismo y cada una de sus actividades. Es la encargada de producir las hormonas que controlan la velocidad con que se queman las calorías y el ritmo de los latidos del corazón. Además,  es productora de proteínas y regula la sensibilidad del cuerpo a otras hormonas.

La glándula tiroidea puede verse afectada por diversos factores, los cuales pueden alterar su funcionamiento y causar varios problemas de salud. Las principales alteraciones funcionales de la glándula tiroidea son el  hipotiroidismo e hipertiroidismo. Ambos casos influyen de distintas maneras en el cuerpo y es muy importante controlarlos para que no afecten la calidad de vida. Una persona sufre de hipotiroidismo si su tiroides produce menos hormonas de lo que el cuerpo necesita, y por otro lado, sufre de hipertiroidismo si su tiroides produce más hormonas de las que el cuerpo necesita.

Es en este marco que el Garvan Institute de Australia decide liberar información sobre distintos análisis realizados a sus pacientes y la condición de su tiroides para que pueda ser investigada la relación entre los mismos.

En particular se investigará la posibilidad de detectar si un paciente sufre de hipotiroidismo.

# Alternativas aplicables de machine learning

Se trata de un problema de clasificación ya que se intenta predecir si un paciente tiene o no la enfermedad a partir de ciertas características del mismo. Además, es un problema supervisado ya que se cuenta con el resultado de la clasificación en los ejemplos del dataset. Por último, otra característica importante del problema es el hecho de que la cantidad de personas que sufren la enfermedad es sensiblemente menor que la cantidad de gente que no la sufre, lo cual debe ser considerado a la hora de elegir un modelo para resolver el problema.

Se evalúa entonces la posibilidad de aplicar alguno de los siguientes modelos de aprendizaje:

* Detección de anomalías por agrupación: estos algoritmos se basan en medir la relación entre distintos ejemplos del dataset según algún criterio (distancias, densidad en el punto, entre otros) y clasificar como outliers a un subconjunto de ellos que se aleje más de las características del conjunto mayoritario.

* Detección de anomalías por métodos de clasificación: existen distintos algoritmos con este enfoque en los que se busca predecir si un nuevo ejemplo es outlier o no a partir de un modelo generado con los datos etiquetados del dataset. Para éste tipo de algoritmos es importante solucionar el problema del desbalance entre la cantidad de ejemplos enfermos y sanos. Algunos ejemplos son Regresión Logística, Support Vector Machines y Árboles de Decisión.

# Descripción del dataset

Para la resolución del problema se utilizó un dataset llamado “allhypo.data” que forma parte de una colección de datasets relacionados con enfermedades de tiroides. Está compuesto por 2.800 registros y 30 atributos que se detallan a continuación:

| Atributo | Faltantes | Min | Máx / Moda | Media | Desvío |
| ---- | :----: | :----: | :----: | :----: | :----: |
| age | 1 | 1 | 455 | 51,844 | 20,461 |
| sex | 110 | - | F | - | - |
| on thyroxine | 0 | - | f | - | - |
| query on thyroxine | 0 | - | f | - | - |
| on antithyroid med | 0 | - | f | - | - |
| sick | 0 | - | f | - | - |
| pregnant | 0 | - | f | - | - |
| thyroid surgery | 0 | - | f | - | - |
| I131 treatment | 0 | - | f | - | - |
| query hypothyroid | 0 | - | f | - | - |
| query hyperthyroid | 0 | - | f | - | - |
| lithium | 0 | - | f | - | - |
| goitre | 0 | - | f | - | - |
| tumor | 0 | - | f | - | - |
| hypopituitary | 0 | - | f | - | - |
| psych | 0 | - | f | - | - |
| TSH | 284 | 0,005 | 478 | 4,672 | 21,449 |
| T3 | 585 | 0,050 | 10,600 | 2,025 | 0,825 |
| TT4 | 184 | 2 | 430 | 109,071 | 35,395 |
| T4U | 297 | 0,310 | 2,120 | 0,998 | 0,194 |
| FTI | 295 | 2 | 395 | 110,788 | 32,884 |
| referral source | 0 | - | other | - | - |
| TSH measured | 0 | - | t | - | - |
| T3 measured | 0 | - | t | - | - |
| TT4 measured | 0 | - | t | - | - |
| FTI measured | 0 | - | t | - | - |
| T4U measured | 0 | - | t | - | - |
| TBG measured | 0 | - | t | - | - |
| TBG | 2800 | - | - | - | - |
| class | 0 | - | negative | - | - |

Por más información, puede consultar la página de la UCI donde se encuentra publicado el dataset bajo el nombre de “Thyroid Disease Data Set¨. El siguiente enlace lo redireccionará a la página antes mencionada.

# Preparación de los datos

Para poder importar el dataset a RapidMiner, herramienta elegida para diseñar el proceso y entrenar el modelo, se debió modificar previamente el archivo “allhypo.data”. Dado que las instancias estaban separadas por un punto, se debió reemplazar cada punto por un “/n” y de esta forma lograr que haya una instancia por línea.

Una vez importado el dataset en RapidMiner, se diseñaron dos procesos de preparación. El primer proceso se utilizó para detectar anomalías con algoritmos no supervisados. Por su parte, el segundo es una continuación del primero y se utilizó para detectar outliers con un algoritmo supervisado.

## Preprocesamiento para algoritmos no supervisados

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Preprocesamiento no supervisado")

En primer lugar se generó un atributo “outlier_label” para utilizarlo en la evaluación del modelo a entrenar. Para esto se utilizó el operador “Generate Attribute” con la siguiente fórmula: si class != false ; true ; false. En otras palabras, “outlier_label” tendrá un valor “true” cuando la variable “class” indique que el paciente representado tiene una enfermedad de tiroides.

En segundo lugar se seteó el atributo generado (outlier_label) como variable de predicción utilizando el operador “Set Role”.

En tercer lugar se eliminaron algunos atributos. El atributo “TBG” se eliminó debido a que todos los registros tenían el mismo valor (false). Por su parte, el atributo “Class” se eliminó debido a que fue reemplazada por el generado (outlier_label). Por último, los atributos “FTI measure”, “T3 measure”, “T4U measure”, “TBG measure”, “TSH measure” y “TT4 measure” se eliminaron por considerarlos irrelevantes (indican si se midió o no). Para esta acción se utilizó el operador “Select Attributes”.

En cuarto lugar se filtraron los registros que tenían valores faltantes en el atributo “T3” (20% del total) y valores inválidos en el atributo “age” (se consideró que no era posible tener una edad mayor a 99 años).

En quinto lugar se convirtieron todos los atributos del tipo nominal a numérico utilizando el componente “Nominal to Numerical”. Esta conversión se debe a que más adelante se aplica PCA y este operador no soporta valores nominales.

En sexto lugar se normalizaron los datos debido a la variabilidad que existía entre atributos y se imputaron los missing values utilizando el operador “Input Missing Values” con un “K-NN” configurado con K = 5, votación por peso activada y función de distancia euclidiana.

En séptimo lugar se aplicó PCA para reducir la cantidad de atributos, considerando que de esta forma el proceso de detección de outliers sería mucho más simple y rápido. Además se utilizó el operador “De Normalize” con el objetivo de generar un modelo que sea utilizado en los algoritmos de detección de outliers.

Por último se utilizaron dos operadores “Store” para guardar el dataset preprocesado y el modelo de normalizado con el objetivo de, no solo utilizarlos en el segundo proceso de preparación, sino que también disminuir tiempos de procesamiento en las futuras etapas.

## Preparación para algoritmos supervisados

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Preprocesamiento supervisado")

Como se mencionó previamente, este proceso de preparación es una continuación del primero, por tanto se comenzó importando, con el operador “Retrieve”, el archivo que contiene el dataset preprocesado por el proceso anterior.

Una vez importado el archivo se procedió a generar un nuevo dataset que contenga 360 registros y presente la misma proporción de outliers y no outliers.

Se utilizó el operador “Multiply” para obtener 2 copias del dataset preprocesado. Sobre la primer copia se filtraron los registros con valor “true” en el atributo “outlier_label”. Sobre la segunda se filtraron los registros con valor “false” en el atributo antes mencionado y se tomó una muestra aleatoria de 180 registros.

Para finalizar se utilizó el operador “Append” para juntar ambas copias y de esta forma igualar las proporciones de outliers y no outliers.

Vale la pena aclarar que, al igual que en el primer proceso de preprocesamiento de los datos, se utilizó el operador “Store” para guardar el dataset preprocesado con el proceso descrito en esta sección, y de esta forma disminuir tiempos en las siguientes etapas.

# Algoritmos evaluados

## Detección de outliers por distancia

El fundamento de éste método es que los datos atípicos se encuentran separados en el espacio de los otros datos del dataset. A cada elemento se le asigna un puntaje que es básicamente la distancia con el elemento k más cercano. Los n elementos con mayor puntaje son potencialmente outliers.

### Parámetros

El valor de n elegido es 180. Fue obtenido gracias a que el dataset ya está etiquetado y permite tener el dato de la proporción de personas que están enfermas al hacerse los análisis. El valor k elegido fue 5 y fue obtenido tras experimentar con todos los valores entre 2 y 10. El otro parámetro es la medida de distancia a utilizar, para el cual se eligió Distancia Euclidiana, porque es la que mejor separación de los datos (y performance) obtuvo.
### Modelo

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Modelo distancia")

### Resultado

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Resultado distancia")

Se obtuvo una precisión de 85.27%, lo que a primera instancia no parece un mal resultado, pero si se observa el valor para el recall en la clase que se busca predecir, se obtuvo solo un 9.44%, es decir, sólo ese porcentaje de los outliers (personas enfermas) fueron detectadas como tal, la cual es una baja performance.


## Detección de outliers por densidad

Los valores atípicos, por definición, ocurren con menos frecuencia en comparación a la normalidad los puntos de datos. Esto significa que en el espacio de los datos atípicos ocupan áreas de baja densidad y puntos de datos normal ocupan zonas de alta densidad.
La densidad es un recuento de los puntos de datos en una unidad normalizada espacio y es inversamente proporcional a la distancia entre puntos de datos. El objetivo de un algoritmo basado en la densidad de outlier es identificar los puntos de datos de áreas de baja densidad.Desde la distancia es la inversa de la densidad, podemos explicar el enfoque de una densidad- basado outlier con dos parámetros, la distancia (d) y la proporción de puntos de datos (p). Un punto X se considera un outlier si al menos p fracción de puntos está más d la distancia desde el punto.

### Parámetros

Para este ejemplo, vamos a identificar la distancia como infinita ya que va desde 0 hasta infinito.Luego la proporción(p) que se especifica es de 76% y del 95% para otra prueba, ya que se recomienda que este valor sea alto.
Dentro de las mediciones de distancia tenemos medición como euclidiano, el coseno , coseno invertido, la distancia al cuadrado o distancia de angulos. El valor predeterminado es euclidiano, y vamos a trabajar con este.
Se testeo con la recomendacion segun el modulo de optimizacion de parametros y tambien con pruebas manuales.

### Modelo

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Modelo densidad 1")

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Modelo densidad 2")

### Resultado

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Resultado densidad")

Ambas pruebas dieron el mismo resultado, con una precisión de 91,8%, no parece un mal resultado, todo lo contrario, pero si se observa el valor para el recall se obtuvo solo un 0.00%, es decir, no se pudieron detectar outliers

Colocando valores de distancias muy pequeños como 2, 3 o 4 , podemos obtener mejores resultados

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Resultado densidad")

Pero esto implica también, a que se consideran outliers datos que no lo son.

## Random Forest
Cuando en el dataset de entrenamiento se cuenta con un atributo que indique si la muestra corresponde a un outlier o no, el problema de detección de outlier se transforma en uno de clasificación tradicional.

Como algoritmo de clasificación se eligió Random Forest ya que corresponde a un tipo de algoritmo de ensamble y esos suelen tener mejor rendimiento que un modelo simple.

### Parámetros
Algunos parámetros a destacar:
* cantidad de árboles: 10
* criterio de división: ratio de ganancia (gain_ratio)
* profundidad máxima: 5
* aplicar prepruning (poda temprana): si

### Modelo

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Modelo random forest")

### Resultado

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Resultado random forest")

# Conclusión

Se pudo observar que los modelos de detección de outliers no fueron exitosos ya que el propio dataset no permite que ninguno de los criterios de detección sea efectivo (se puede observar que los ejemplos con la enfermedad están muy cercanos a los que no la tienen). Por esa razón se decidió utilizar el modelo de Random Forest ya que se obtuvieron los mejores resultados de recall en la clase que se interesa predecir (verdadero-positivo). Además se trata de un modelo liviano a la hora de integrarlo en un sistema de apoyo a las decisiones médicas.

Los resultados son moderadamente aceptables si se considera como un problema de clasificación, pero si se toma en cuenta que la clase que se predice tiene una baja representación dentro del dataset, el resultado puede ser considerado bueno.

# Presentacion

Descargue la presentacion con imagenes del proceso haciendo click en el siguiente [enlace](Deteccion de anomalías.pdf)
