# Características del problema

Las enfermedades cardiacas en general se tratan de trastorno del corazón y los vasos sanguíneos, dentro de las cuales podemos tener un listado amplio de categorías con sus características específicas, como por ejemplo insuficiencia cardiaca, cardiopatía isquémica , infarto de miocardio,muerte súbita , entre otras. Todo el conjunto de estos problemas son de los tipos de enfermedades con más alta prevalencia en la población, siendo una de las mayores causa de muerte o discapacitación del individuo, según la organización mundial de la salud se ha estimado que 17.5 millones de muertes anuales ocurren por esta causa, lo que representa un 31% de del total de muertes registradas , debido a esto un gran número de los gastos en el sector de la salud son relacionados a tratar este tipo de patologías. Este sector se dice que es muy rico en información pero muy pobre en conocimientos, dado que se posee mucha data sobre los pacientes, pero no esta procesada. Se podría concluir que esto puede ocurrir dado que el procesamiento de esta informacion es muy delicada ya que no solo son datos confidenciales, sino que afectar directamente al paciente y obviamente al profesional que con la ayuda de esta informacion va a construir un diagnóstico y un posible tratamiento del mismo, por lo que un mal analisis de esta informacion , puede modificar de forma radical la vida del enfermo y del médico tratante.
El objetivo de este estudio es poder investigar los factores significativos que contribuyen en la detección de presentes enfermedades del corazón y futuras , con el fin de poder prevenir , diagnosticar y poder formular un plan de acción. Se busca poder explorar los patrones ocultos de los conjunto de datos , y de esta forma detectar de manera temprana enfermedades como infarto , miocardiopatía dilatada , cardiopatía isquémica , etc.
Para estos estudios se pueden considerar múltiples caracteristicas tanto físicas como sociales de los individuos , como lugar donde vive, ritmo de vida, niveles de colesterol, edad, sexo, hipertensión, obesidad, consumo de alcohol,etc.
En esta instancia utilizaremos lógica de regresión para investigar las características que contribuyen a las enfermedades cardiacas , de esta forma examinaremos si las personas son propensas a tener o no este problema evaluando 76 atributos relacionados, sin embargo se podría aplicar muchas técnicas más , con el fin de construir un sistema eficiente y preciso.
En conclusión, el estudio de esta caracteristicas puede ser de mucho valor no solo para el control de recursos económicos tanto para las clínicas y hospitales sino también para el mismo enfermo. Desde el punto de vista económico pero también desde el marco de su salud dado que muchas enfermedades cardiacas los pacientes no presentan síntomas concretos que puedan determinar su posible enfermedad ,por lo tanto a través de estos medios se podría encontrar características que colocándolas bajo un contexto , sea posible detectar estos problemas.

# Proceso de RapidMiner
Carga de Datasets
Los datos para el proceso se obtienen de cuatro datasets pertenecientes a la colección de UCI “Heart Disease DataSet”; el mismo se puede encontrar en el siguiente enlace.

Los datasets provienen de la siguientes instituciones:
Cleveland Clinic Foundation (cleveland.data)
Hungarian Institute of Cardiology, Budapest (hungarian.data)
V.A. Medical Center, Long Beach, CA (long-beach-va.data)
University Hospital, Zurich, Switzerland (switzerland.data)

Estos se cargan en RapidMiner utilizando el operador Retrieve y se unen con un operador Append.

## Tratamiento de atributos

Luego de un análisis de los atributos (features) del dataset, se decidió seleccionar los siguientes atributos para utilizar en el proceso:
* age
* chol
* cp
* cxmain
* exang
* fbs
* smoke
* htn
* laddist
* ladprox
* lmt
* om1
* painexer
* rcadist
* rcaprox
* num
* relres
* restecg
* sex
* trestbpd
* trestbps


A su vez, detectamos que se pueden combinar los atributos “smoke”, “years” y “cigs” en un atributo que denominamos “smoke_gen”. De esta manera logramos dos cosas: 1) reducir en dos las cantidad de features a analizar y 2) tener un atributo significativo para procesar que, de otra manera, no se podría utilizar debido a la gran cantidad de valores faltantes (missing values) que posee cada atributo por separado.

## Manejo de Missing Data
En los datasets originales, los valores faltantes se encuentran identificados con el valor “-9”. Utilizamos el bloque Declare Missing Value para transformar estos valores en valores faltantes que la herramienta puede entender.

Luego, también se utiliza un algoritmo de k-NN (vecinos más cercanos) con valor k=1. Esto comparada las filas con atributos faltantes con los demás ejemplos y coloca, para cada atributo faltante, el valor de su ejemplo más similar. Para comparar el grado de similitud, se utiliza como función de distancia Euclidiana.

## Algoritmo de ML

Considerando que el problema se trataba de uno de regresión logística, se eligió como algoritmo de ML el de Regresión Logística Lineal. Como algoritmo de optimización para el mismo se utilizó (L-BGFS).

Para hacer que el algoritmo converja más rápidamente durante el entrenamiento, se normalizaron los conjuntos de entrenamiento y test con una transformación Z, que hace que cada atributo tenga una media de 0 y un desvío estándar de 1.

## Validación
Para separar el conjunto original en dos conjuntos, uno para entrenamiento y otro para testing, se utiliza el método de Validación Cruzada con 10 dobladillos (10-fold Cross Validation), dejando un 70% de cada dobladillo para entrenamiento y el restante 30% para training.

# Conclusiones

Se logró un modelo que permite predecir si una persona posee una enfermedad cardiovascular con un 87,98% de confianza.
Se utilizaron varias formas de obtener el conjunto de atributos los cuales determinarían el resultado y pudimos constatar que el porcentaje de confianza no variaba demasiado.

# Presentación

Descargue la presentación con imágenes del proceso haciendo click en el siguiente [enlace](Heart Disease.pdf).

Enlaces
### [Descargar Modelo Rapidminer](heart-disease-process.rmp)
