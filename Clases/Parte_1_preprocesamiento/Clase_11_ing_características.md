Clase del 7 de mayo. Todo lo que se presenta en estas notas es lo mismo que hay en las dispositivas más ideas adicionales que se dieron durante la clase. 

# Ingeniería de Características (Feature Engineering)

Es el ejercicio de hacer transformaciones y contrucción de variables nuevas a partir de las variables originales con el fin de mejorar el desempeño de mis modelos, muchas veces por ejemplo cambiar el dominio de mis variables y mejorar el desempeño de mi modelo o mejorar la interpretabilidad entre mis variables y la variable respuesta. 

Ejemplos de transformaciones: 
* Discretización. 
* Normalización
* Binning

Ejemplos de casos de uso: 
* En algunos casos que se usan unos algoritmos específicos de machine learnging, estos piden que los datos estén ESCALADOS - valores entre 0 y 1 o -1 y 1 - pues porque al ser algoritmos de distancias pueden tener diferentes pesos por esa diferencias de escalas y generar sesgos.

* Dentro del KDD que es el Proceso de Descubrimiento de Conocimiento se corresponde con la etapa de transformación de datos.

    * La transformación de datos engloba, en realidad, cualquier proceso que modifique la fora de los datos. 

    * Prácticamente todos los proceso de preparación de datos entrañan algún tipo de transformación. 

La principal tarea del Feature Engineering es la tarea de mejorar el rendimiento del modelado en un conjunto de datos mediante la transformación de su feature space (llevar le dominio del conjunto de características a un nuevo domino para ayudarme a mejorar ese desempeño). 

## Herramientas para la ingeniería de características

### 1. Normalización ---
La normalización consiste en escalas los features (numéricos) de manea que puedan ser mapeados a un rango más pequeño. 

Por ejemplo: (0 a 1) ó (-1 a 1). 

La normalización es particularmente utilizada en: 

* Tareas de data mining donde las unidades de medidas dificultan la comparación de features. 

* Medidas de Distancias. Vecinos más cercanos, clustering, etc. Pues ayuda a evitar que atributos con mayores magnitudes tengan un mayor peso que los rangos pequeños. 

Los métodos más utilizados para normalizar son: 

**1. MIN-MAX**

**2. Z-SCORE (también sirve para la identificación de datos atípicos)**

**3. DECIMAL SCALING**

#### 1. Normalización MIN-MAX
Lo que hace esta normalización es analizar la variable numérica con la que se esta trabajando y encontrar el valor mínimo y máximo para esta variable y la nueva variable TRANSFORMADA es : 

$$X^*_{mm}= \frac{X - min(X)}{range(X)} = \frac{ X -min}{max(X)-min(X)}$$

Y por la forma que se tiene, entonces los valores de esta normalización **VAN DE 0 A 1**.

Este método requiere conocer el mínimo y el máximo, y que estos datos sean los mismos tanto para el entrenamiento del modelo como para el test de este. Pues, si se llega a tener un valor máximo y mínimo diferentes puede generar malas predicciones y por ende malas métricas. (Si el testeo tiene valores más granges, seguramente se caerá el desempeño)

CONCEPTO IMPORTANTE
**PSI (Population Stability Index)**: Es una métrica que establece la similitud entre la distribución de una variable X en el entrenamiento con ella misma en el test; es decir, simplemente mide que las distribuciones sean similares. Si las distribuciones cambian drásticamente el modelo no va a servir pues este solo entiende una distribución específica, con la que se entrenó. (ESTO ÚLTIMO ES LO QUE SE CONOCE EN LA PRÁCTICA COMO QUE EL MODELO SE DESCALIBRÓ y se debe volver a hacer o re-entrenar pues hay un cambio en la distribución de los datos)

#### 2. Normalización Z-score
Como vimos anteriormente en el análisis de datos atípicos. Los valores para una variable $X$, se normalizan en base a la media y desviación estandar de $X$:

$$Z-score = \frac{X - mean(X)}{sd(X)}$$

Se crea una nueva variable Z que tiene los valores de X transformaddos con esa fórmula.

Si la distribución de los datos es simétrica, se moverá entre -3 y 3 aproximadamente. Pues es llevar a que los datos tengan una distribución N(0,1). En caso que la distribución sea bastante asimétrica usamos el Z-score modificado que usa estadísticos robustos a los datos atípicos como mediana y el mad.  

#### 3. Normalización Decimal Scaling
Decimal Scaling asegura que cada valor normalizado **se encuentra entre -1 y 1**. 

$$X_{decimal} = \frac{X}{10^d}$$

Donde $d$ representa el númerod de dígitos en los valores de la variable con el valor absoluto más grande.

Por ejemplo, si tengo una variable con los números del -99 hata el 99, el número de dígitos en los valores de la variable con el valor absoluto más grande sería 2, $10^2=100$ y cada número entonce se divide entre 100. 

$$\frac{99}{100}=0.99$$ $$\frac{-99}{100}=-0.99$$

![Normalización ejemplos](../imagenes/normalizacon_ejemplos.png)

Notese que estos métodos no cambian nada en la distribución, conservan el sesgo, la variabilidad y toda su característica, lo único que cambia es directamente la escala de los datos. 

### 2. Escalados Robustos

* Para data frames con muchos valores atípicos, un escalado usando la media y la varianza de los datos no funcionará muy bien. 

* En est caso se puede usar un método robusto como reemplazo. 

* usan estimaciones más sólidas para el centro y el rango de sus datos. 

* Por ejemplo: Mediana (o algún percentil) e IQR.

Un escalado robusto es por ejemplo el **Z-score modificado**

### 3. Técnicas para lograr normalidad.

Con los escalados que acabamos de ver sabemos que la distribución no cambia, pero hay casos que es necesario cambiar la distribución para que sea más simétrica. 

En casos de algoritmo paramétricos que tengan como supuesto la distribución normal, esto puede ser una gran ayuda. 

$$sesgo = \frac{3*(media - mediana)}{desviación}$$

* Si la media es mayor a la mediana hay sesgo positivo (a la derecha). 

* Si la media es menor que la mediana entonces hay sesgo negativo (a la izquierda).

Entonces hay un tipo de transformaciones para reducir este sesgo y tratar de convertir la distribución de la variable un poco más simétrica:


(para variables positivas)
* 1. Raíz cuadrada. 
* 2. Logaritmos.
* 3. Inversa de la Raíz Cuadrada.

Aunque si se esta usando un algoritmo no paramétrico que no necesita la normalidad pues directamente es mejor usarla original. 

![sesgo ejemplos](../imagenes/sesgo_ejemplo.png)

No se trata de normalizción en el sentdio de llevar a una escala específica sino másb bien llevarla a que se parezca a una distribución normal, simétrica. 

### 4. Discretización

* Es una técnica sencilla pero demasiado útil y es dividir el rango de una variable continua en intervalos. 

* Es llevar una variable continua a que sea categórica. 

Ahora, ¿Como establecer estos rangos de discretización?

#### 1. Discretización Binnig.



