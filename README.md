# TP-Analitica-Descriptiva

El presente proyecto consiste en un análisis del mercado inmobiliario con el objetivo final de un modelo de regresión que sirva para predecir los precios de las propiedades. 

El mercado inmobiliario es altamente competitivo y volátil, con gran heterogeneidad entre distintas comunidades. Inmobiliarias y desarrolladoras necesitan información precisa para fijar precios de publicación, evaluar oportunidades de inversión y analizar tendencias de oferta. Si los precios se basan en operaciones anteriores de similares características y no en la mera voluntad del vendedor las partes obtendrán un trato más justo (interés del gobierno en busca de un equilibrio general) y las inmobiliarias estarán más informadas para tomar decisiones.

Nuestro foco de estudio está en las operaciones de la plataforma inmobiliaria Lianjia y los potenciales clientes que navegan en ella. En particular, con este trabajo buscamos satisfacer a ambas partes. Primero, satisfacer el interés de Lianjia en identificar el tipo de propiedades que mejor se venden y el precio de venta de las propiedades publicadas, con el fin de visibilizar en la plataforma a aquellas propiedades que más rédito le van a generar a la compañía. Desde el lado del comprador y cliente inmobiliario, esté análisis busca poder identificar oportunidades en el mercado de real estate.

El dataset utilizado se denomina  ["Housing price in Beijing"](https://www.kaggle.com/datasets/ruiqurm/lianjia) y abarca la venta de propiedades en Beijing principalmente durante los años 2011-2017, sin extenderse a propiedades en alquiler. Además de este dataset, complementaremos el análisis con datos de la ubicación de las estaciones de subte de la ciudad, facilitadas a partir del scrapeo de esos datos de parte de un usuario de GitHub. El dataset y el scrapeo de donde se obtuve en este [link](https://github.com/nomoreoneday/Beijing_Subway_route_seach_agent/blob/master/subway.txt).

A partir del análisis del mismo buscamos responder algunas preguntas e hipótesis que guían nuestro análisis.
Las hipótesis principales que nos surgieron al respecto son:
- Hipótesis 1: Los departamentos de menos ambientes presentan un precio por m² más alto que los de mayor tamaño.
- Hipótesis 2: La superficie es el factor principal del precio.
- Hipótesis 3: La condición de renovación tiene un gran impacto en el precio, pero no del precio por m².
- Hipótesis 4: La cantidad de seguidores tiene una alta correlacion con el precio por m².
- Hipótesis 5: El impacto en el precio por m² de la cercania al metro es más alta en departamentos chicos que grandes.
- Hipotesis 6: hay determinados tipos de construcciones/edificios que solo se relacionan con ciertos rangos de construcción de los edificios

Estas nacen de distintas preguntas en diferenets niveles analíticos que orientaron las mismas. Tales son:
- En lo descriptivo:
¿Cuál es el precio promedio por m² en cada barrio (de ser posible de agrupar las comunidades en barrios)? ¿Cómo se distribuye la oferta por cantidad de ambientes? ¿Tener más habitaciones está vinculado a un mayor precio por m²?
- En el diagnóstico:
¿Por qué ciertos barrios/comunidades presentan precios más altos? ¿Cómo influyen las distintas variables en el valor de un departamento?
- En lo predictivo:
¿Se puede predecir el precio por m² de un departamento en dólares a partir de sus características? ¿Debería Lianjia actuar de manera especial con las propiedades que llevan muchos días sin venderse?
- En lo prescriptivo:
¿En qué barrios conviene publicar propiedades nuevas para maximizar rentabilidad? ¿Qué tipos de propiedades tienen mayor atractivo? ¿Podríamos sugerir la construcción de propiedades que nos aseguran precios de venta altos?

En este repositorio se encuentran todos los archivos necesarios. En la carpeta "data" están los dos datasets. El dataset "Copia de new.csv" es una versión reducida del dataset de ventas de propiedades inmobiliarias ya que por su tamaño no es posible cargarlo en GitHub. Para accederlo de manera completa puedan acceder a él en el link de Kaggle mencionada anteriormente. En esa misma carpeta está también "subway.txt" que contiene los datos de las estaciones de subte de Beijing. Por otro lado, para encontrar la notebook con el código correspondiente pueden entrar al archivo "TP_Descriptiva.ipynb" en la carpeta "notebooks".

A fin de cuentas, el objetivo es identificar patrones de precios y oferta inmobiliaria en Beijing en el periodo 2011-2017, para aportar información estratégica a inmobiliarias como Lianjia y desarrolladores en la toma de decisiones de pricing y segmentación y poder ofrecerle servicios de a clientes de interés sobre oportunidades en el rubro.

## Limpieza y tratamiento de datos

Se realizó una limpieza inicial del dataset para eliminar filas con valores incoherentes o caracteres no válidos, asegurando formatos consistentes en todas las columnas.  
Se corrigieron tipos de datos, se recalculó la variable `totalPrice` y se eliminaron duplicados.  
Las columnas con proporciones elevadas de datos faltantes, como **DOM**, fueron removidas, mientras que otras de relevancia analítica, como **constructionTime**, se conservaron tras reemplazar valores desconocidos por `NaN` para su posterior imputación.

---

## Análisis y tratamiento de outliers

El análisis exploratorio evidenció variables con distribuciones no normales y presencia de valores extremos.  
Se decidió conservar los outliers leves al considerarse representativos del mercado, y **ajustar únicamente los valores severos**: truncando los extremos superiores o reemplazando en variables discretas por un máximo razonable.  
Se eliminaron pocos casos con precios o fechas incoherentes, y se incorporó una nueva variable —**distancia al subte**— que enriquece el análisis de precios por localización.

---

## Manejo de valores faltantes

Se aplicaron pruebas estadísticas para identificar el tipo de ausencia.  
El **test de Little** permitió descartar la aleatoriedad completa (MCAR), y el análisis de regresiones logísticas mostró que los faltantes dependían de otras variables, por lo que se clasificaron como **MAR**.  
En consecuencia, se imputaron mediante el método **KNN**, manteniendo la coherencia multivariada del conjunto.  
Finalmente, se ajustaron los valores imputados (por ejemplo, redondeando *buildingType* a enteros) para conservar su validez semántica.

## Tests de hipótesis

Con el dataset limpio e imputado, se contrastaron las hipótesis planteadas mediante pruebas estadísticas adecuadas según el tipo de variable y los supuestos de normalidad.  
Se aplicaron comparaciones de medianas (Kruskal–Wallis y test de Dunn), regresiones lineales simples y múltiples, y un test de Chi-cuadrado para variables categóricas.

En síntesis:
- Los **departamentos más pequeños** tienden a tener un **precio por m² más alto**, aunque la diferencia no es significativa en todos los casos.  
- La **superficie** resultó ser el **principal determinante del precio total**, confirmando su relevancia en el modelo.  
- La **condición de renovación**, la **cercanía al metro** y la **cantidad de seguidores** mostraron relaciones **positivas y estadísticamente significativas** con el precio.  
- Se verificó también una **asociación entre el tipo de edificio y su rango de antigüedad**, lo que sugiere segmentación dentro del mercado.

Estos resultados respaldan la mayoría de las hipótesis formuladas y refuerzan la importancia de los factores estructurales y de localización en la formación de precios inmobiliarios en Beijing.


