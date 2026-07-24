# Pesar términos y comparar documentos
Anteriormente entendimos el modelo de bolsa de palabras que es la representación de la frecuencia de las palabras con la matriz término-documento y es una representación útil para pasar texto a número para usarlos por medio de modelos.

Ahora bien, sabemos que si una palabra tiene una frecuencia muy alta no necesariamente quiere decir sea importante o informativa; entonces, ¿cómo podemos medir la importancia de esas palabras?

A continuación presentaremos como asignar una importancia que nos permita comparar documentos. Por tanto, se verá la representación de documentos, ponderación de palabras y como pueden compararse textos dado lo anterior.

## TF-IDF
Es un separador del peso de las palabras que intenta resolver el problema de las palabras vacías y darle mayor peso a las palabras que sean frecuentes pero no comunes; trata de identificar las palabras que verdaderamente son significativas. **TF midiendo la importancia local dentro de un solo documento** y el **IDF es que un término me permita diferenciar entre esos documentos.**

### TF (frecuencia de términos)
Importancia local de documentos, mide que tan seguido aparece un término $t$ - en un documento $d$. Es la frecuencia cruda(sin importar si es importante o no) de cada una de los términos. 

También se tienen variantes habituales:

* Conteo crudo: Número de ocurrencias $t$ en $d$. 

* Una frecuencia relativa: Como $\frac{\text{Ocurrencias}}{\text{Total de palabras en d}}$

* Log-normalizada: $1 + log(conteo)$ para amortiguar conteos simples ya que muchas veces una alta frecuencia no es importancia.

### IDF(Frecuencia inversa de documento)
Lo que hace es ver que tan rara es una palabra dentro de todo el CORPUS. Una palabra rara difernciar un texto de otro; son muy informativas.

$$idf(t) = log(\frac{N}{df(t)})$$

Donde,

* $N = \text{número total de documentos}$

* $df(t) = \text{documentos que contienen t}$. 

**Conexión con la ley de Zipf**: Las pocas palabras muy frecuentes son justamente las menos informativas. IDF las penaliza de forma natural - es la versión gradual de quitar stopwords.


### TF-IDF: el producto

**Por tanto TF-IDF lo que hace es un balance entre la importancia de una palabra en el documento contra que tan distintivo es cada término dentro del corpus -- importancia local contra global--.**

$$tfidf(t, d) = tf(t, d) x idf(t)$$


Alto cuando el término es frecuente en este documento pero raro en el corpus: cuando es característico de ese documentos. 

Ej: 
* "tango" aparece en los 5 documentos $\rightarrow$ df = 5 $\rightarrow$ idf = log(5/5) = 0. A pesar de ser el tema, su peso TF-IDF es nulo porque no permite distinguir documentos.

* "Gardel", "sueldos","vendido" aparecen en 1 solo documentos $\rightarrow$ IDF alto $\rightarrow$ caracterizan a su texto.

## Documentos como vectores
Luego de calcular el TF-IDF cada documento va a quedar representado como una fila de números que puede interpretarse como un vector; es la representación matriz término-documento.Pero en lugar de tener 1 y 0 para ver si la palabra está o no, se tenrá el valor del TF-IDF de cada palabra.

Cada palabra es una dimensión (VARIABLES) y cada documento es un punto o vector en el espacio; los vectores se pueden comparar entre sí midiendo distancias en el espacio, como medidas de similitud entre documentos.

### Medidas de similitud entre vectores(documentos)

#### Similitud Coseno
La similitud de los vectores se puede medir con el ángulo entre los vectores sin importar el tamaño; de tal manera que si tienen términos muy parecidos entonces los documentos van a ser más similares porque apuntan a la misma dirección. 

$$sim(A,B) = cos(\theta) = \frac{(A ° B)}{||A||*||B||}$$

Recordemos que el producto punto representa el grado de alineación y escala mutua de dos vectores, midiendo que tanto apuntan a en la misma dirección y dividirlo entre el producto de sus magnitudes es normalizarlo, así vale 1 si apuntan a la misma dirección, 0 si son ortogonales.

## Aplicación: buscar y detectar duplicados

**Un buscador minúsculo**
1. Vectorizar la consulta con el mismo esquema TF-IDF. 
2. Calcular la similitud coseno entre la consulta y cada documento. 
3. Ordenar de mayor a menor similitud y devolver los primeros

**Detección de plagio/casi-duplicado**
Dos documentos con similitud coseno por encima de un umbral son sospchosos de compartir contenido. La misma idea sirve para agrupar noticias repetidas.

Ahora pasemos al notebook para ver esto de una manera práctica. 