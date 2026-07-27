# Modelos de lenguaje e IA generativa

¿Donde quedamos?
Lo último que se vió es que la atención produce representaciontes textuales lo cual es la solución a los problemas de la definición estática de los embeddings (representación de un palabra en un vector denso en un espacio vectorial) y que los modelos decoder(GPT) sirven para generar texto. 

Ahora bien, si el modelo tiene una buena represetación del contexto gracias a la atención podemos responder una pregunta: ¿Qué palabra viene después? entonces ahora podemos establecer probabilidades a partir de los distintos contextos. Esto es lo que hay detrás de los modelos de IA generativa. 

Un modelo de lenguaje asigna probabilidadesd a muchas palabras posibles. Dependiendo de cada contexto hay una distribución de probabilidad parar todas las palabras posibles después de ese contexto por lo que puede ser bastante pesado. 

Ojo, la salida no siempre es verdad siempre sino que es una palabra predicha de acuerdo al contexto. Es un modelo bastante y grande y complejo. Acá no se está presentando formalmente la matemática que tiene por debajo. Más adelante se presentará esto. 

## ¿Qué es un modelo de lenguaje?
Un modelo que predice o modela palabras, con el podemos hacer traducción, responder preguntas, responder preguntas, etc. 

(LLM: Un grán modelo del lenguaje que permite hacer todo esto, hay modelos preentrenados que son fáciles de usar. Con una cantidad increible de parámetros y lo cual es demasiado difícil crear desde cero, por ello, es necesario usar modelos preentrenados para aprender.

Hoy en día ya no se construye el modelo de lenguaje desde cero, sino que se están usando agentes para hacer todo. Modelos de agentes o modelos preentrenados de hugging face)

Un modelo de lenguaje responde la pregunta: 

$$p(\text{palabra}|\text{contexto previo})$$

Una distribución de probabilidad sobre la siguiente palabra, dado lo que vino antes.

Ej: "El estudiante entrengó el __"
Ahí saca una distibución de probabildiad para ver cual es la palabra más probabble, por ejemplo puede ser "trabajo"  entonces ahora nuestra frase es: " El estudiante entregó el trabajo __" y se vuelve a crear de nuevo otra distribución a partir de nuestro nuevo contexto previo. Es un proceso cíclico de predecir y completar. 

**Conexión con el modelo de bolsa de palabras:** Allí se preguntaba acerca de la probabilidad de sacar la palabra 'tango' de la bolsa. Aquí se pregunta acerca de la probabilidad de la siguiente paalbra dado el contexto. Es la misma pregunta probabilística pero ahora acondicionada al contexto. 

**TAREA PARA MANUEL: A PARTIR DE UN TEXTO, CLASIFICARLO: SI ME DICEN QUE LA RAZÓN DE QUE MURIO ES POR X, ENTOCNES LA RAZÓN PERTENECE A LA CATEGORÍA Y(automovilísticos, enfermedad, natural, violencia). ESCOGER LAS CATEGORÍAS PREVIAMENTE y usar un modelo de clasificación crear una base de entrenamiento bien etiquetada y hacer todo el proceso. n-gramas**

## N-gramas clásico
secuencia contigua de n elementos (como palabras, letras o sílabas) extraída de un texto(corpus), cuyos tipos principales son unigramas, bigramas y trigramas. En ese sentido, tiene un contexto corto y se pueden usar para hacer generación de texto, acá pues el contexto es de muy pocas palabras. 

En un transformer puede incluir cientos y miles de tokens. 

#### n-gramas (clásico):
* Predicen según las últimas n-1 palabras. 
* Simples y transparentes.
* No capturan texto largo ni significado. 

Unigrama: "mineria" predicción:datos

Bigramas: "mineria de" $P(\text{palabra}|\text{palabra anterior})$

Trigrama: "mineria de datos" $P(\text{palabra}|\text{2 palabras anteriores})$

Por tanto, como se tienen contextos tan pequeños entonces la predicción no es tan confiable como lo es la predicción de un transformer. Funciona para frases cortas pero no captura relaciones largas, luego, su evolución es el transformer que son una estructura más compleja usando el mecanísmo de atención pues permite tener esas relaciones largas.


#### Transformers (moderno):
* Usan atención sobre todo el contexto. 
* Capturan significado y dependencias largas. 
* Escalan a enormes corpus.


#### Conceptos importantes

**Generación autorregresivo**: Predicen una palabra, la agregan al contexto y repiten.

La **temperatura** controla cuánta aleatoriedad hay en cada elección de palabras para el modelo; es decir, si se escoge la palabra más probable o se permite mayor aletoriedad. Es la cantidad de variablidad que se está dispuesto a poner en el modelo: 

* **Baja temperatura**: Texto conservador, repetitivo y coherente (la palabra más probabile siempre).

* **Alta temperatura**: Hace que el modelo sea más creativo pero más riesgoso; osea, mayor posiblidad de alucinación.

Esto puede elegirse de acuerdo a la población objetivo del modelo de lenguaje, por ejemplo para niños o personas de bajo IQ puede ser difícil entender. 

Ejemplo con la frase: "la mineria de datos"

* **Baja temperatura**: "La minería de datos permite analizar grandes volúmenes de información"

* **Alta temperatura**: "La minería de datos canta patrones invisibles en océanos de información" 

Por tanto, esa temperatura no hace que cambie lo que el modelo sabe pues ya están las distribuciones establecidas, sino que lo que cambia es el muestreo que se hace; es decir, no hay un re-entrenamiento del modelo sino que solo modifica la forma de escoger la siguiente palabra. 


vectores (modelo de bolsa de palabras, TF-IDF) $\rightarrow$ significado(embeddings y espacios latentes)$\rightarrow$ contexto(mecanismo de atención) $\rightarrow$ predicción $=$ IA generativa.

Por tanto, la IA generativa es una consecuencia estadística y computacional que requiere muchos datos, arquitectura, entrenamiento costoso y diseño. 

#### Mirada crítica
* Alucionaciones: Es la información falsa predicha por el modelo fluidéz en un texto no necesariamente es veracidad. Todo se basa en probabilidades, no piensa y por eso alucinaban. 

* Sesgos: El modelo hereda los sesgos del corús con que se entrenó

* Saber que debajo hay vectores y probabilidades esto nos permite usar estas herramientas con criterios, no como una caja negra mágica. No hay que confiar siegamente en la IA.  