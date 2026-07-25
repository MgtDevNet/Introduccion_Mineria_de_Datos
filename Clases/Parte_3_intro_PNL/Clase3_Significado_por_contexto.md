# Significado por contexto

Con lo visto hasta ahora podemos comparar documentos con la similitud coseno sobre vectores TF-IDF. Pero esos vectores tiene un problema de significado.

**El problema concreto:** Los vectores de perro y gato son ortogonales, igual que perro y avión. Por tanto, la representación no captura que perro y gato son cercanos. Esta metodología no entiende de contexto. 

## La hipótesis distribucional y Embeddings
Las palabras que aparecen en contextos parecidos tienden a significar cosas parecídas. "Café" y "té" aparecen rodeadas de palabras similares (beber, taza, caliente, mañana). 

La idea es la misma intuición que usaron NER y POS tagging: el contexto define el rol y el sentido de una palabra. Aquí la llevamos un paso más allá: el contexto define su significado completo, y lo codificamos como un vector. 

Acá es donde pasamos de matrices dispersas (llenas de ceros) a vectores densos porque todas las palabras tiene un peso asociado de acuerdo a la relación de una palabra con la otra. Esa es la forma de darle a entender a un computador que hay un contexto detrás de cada palabra.  

**TF-IDF (DISPERSO)**
* Miles de dimensiones (una por término).
* Casi todo ceros. 
* Sin relación entre dimensiones.

**EMBEDDING (DENSO)**
* 100-300 dimensiones
* Valores continuos, todos usados. 
* Dimensiones latentes que capturan significado.

Por tanto, un embedding es un vector denso de valores continuos que son pesos para dar contexto para palabras. 

Es una reducción de dimensionalidad. Cada nueva dimensión combina información de muchos términos del vocabulario original y capturan un "tema" o "concepto" oculto en el corpus. Palabras que antes eran ortogonales quedan cerca si comparten esos conceptos. Esto se llama **dimensiones latentes**

Ejemplo: entiende que perro se ascia con gato, veterinario, animal, mascota, etc. Algo que la matriz TF-IDF nisiquiera entiende. 

### word2vec: aprender significado
Dos formas para hacer word-embeddings, los algoritmos: 

1. **CBOW**: Predecir una palabra a partir de su contexto. Como un tipo de algoritmo de vecinos más cercanos. 
 
2. **Skip-gram**: Predecir el contexto a partir de una palabra. 

Los vectores aprenden relaciones semánticas que se pueden operar com aritmética: 

rey - hombre + mujer = reina

Y luego de pasar de un espacio disperso a un espacio denso, la forma  de ver si 2 embeddings son parecidos es con la similitud coseno como haciamos con los vectores de la matriz dispersa TF-IDF

## Problema de los embeddings
El problema de los embeddings es que capturan un significado estático; es decir, Banco para sentarse y Banco financiero colapsan en el mismo vector sin importar la frase.  

la atención y la arquitectura transformer es lo que permite que las computadoras entiendan mejor el contexto de las palabras. 

"atention is all you need"

