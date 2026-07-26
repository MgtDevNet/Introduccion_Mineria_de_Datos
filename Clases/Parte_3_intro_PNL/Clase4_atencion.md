# Representación contextual
#### Atención y transformers, sin matemática pesada. 

Anteriormente lo que vismo fue la construcción de la matríz término documento con el modelo de bolsa de palabras, luego aprendimos a calcular la matriz término-documento pero con la formula TF-IDF para tener una representación de frecuencia e importancia de una palabra dentro de un corpus. Podíamos saber las relaciones entre documentos con la similitud coseno pero no había forma de tener contexto, es decir, es un análisis vacío. Luego, llegaron los embeddings para con una idea similar a la reducción de dimensionalidad, transformar ese vector que representa una palabra en un nuevo espacio denso que le da pesos a las palabras que lo rodean y permiten dar un contexto pero hasta cierto punto se quedaba corto pues da significados estáticos a una misma palabra, lo cual no sucede en le mundo real. 

Por ejemplo: La palabra banco puede tener 2 usos. 
* Banco para sentarse. 
* Bando para retirar dinero. 
Tiene un significado intrínseco bastasnte diferente pero que es representado por lo mismo en los embeddings.

Por ello, se necesita una estrategia que permita diferenciar estas palabras según el contexto. 

lo que hacíamos con los embeddings era convertir las palabras en vectores densos lo que es un avance frente al TF-IDF ya que palabras relacionadas podían quedar cerca, pero cada palabra tiene un único vector que la representa sin importar la frase y ese es el problema.

## Mecanismo de atención
Nota: Los conceptos que se vean serán una sobre-simplificación de la matemática que tiene en su interior. 

**Intución**
Cada palabra de la frase "mira" a todas las demás y decide a cuales y prestar ATENTCIÓN. Luego se reexpresa combinando la infomación de las palabras relevantes para ella. 

**Ejemplo: Para entender "banco", la paalbra mira a "parque" o "dinero" en la misma frase. Según cual esté presente, su representación cambia y así es como hay una representación contextual.**

lo cual es un avance frente a los embeddings.

## Atención en 3 pasos (sin fórmulas)
1. Comparar: Cada palabra mide cuánto se relaciona con cada palabra de la frase. 

2 Ponderar: Esas relaciones se convierten en pesos que suman 1 y con ello decidir a quién prestar más o menos atención.

3. Combinar: Cada palabra se vuelve un promedio ponderado de las demás

Por ejemplo: 

Frase: "fui al banco a sacar dinero" entonces por ejemplo para el vector de la palabra "banco" (el proceso se hace con cada palabra).

1. Comparación

| Palabra raíz | Subpalabra | Clasificación |
| :--- | :--- | :--- |
| banco | fui | no es informativa |
| banco | al | no es informativa |
| banco | banco | puede ser informativa |
| banco | a | no es informativa |
| banco | sacar | es informativa |
| banco | dinero | no es informativa |

2. Ponderación. Los pesos deben sumar 1 y pues a mayor peso mayor importancia de contextualización para las palabras.

| Palabra raíz | Subpalabra | Ponderación |
| :--- | :--- | :--- |
| banco | fui | 0.05 |
| banco | al | 0.02 |
| banco | banco | 0.1 |
| banco | a | 0.03 |
| banco | sacar | 0.3 |
| banco | dinero | 0.5 |

Por ejemplo en este caso a las palabras que más peso se le dio fue "sacar" y "dinero" pues son las que me dan mayor contexto y eso se hace por medio de un algoritmo que se llama `softmax`. Y en eso se basa repartir al atención y creación de los pesos. 

3. Combinación. No solo ya se decidió que palabras son más importantes sino que también se crea un nuevo vector. 

antes: banco = [,...,]
ahora: banco = 50%dinero + 30%sacar + ... + 5%fui. Esto es la ponderación de los pesos para la representación de la palabra teniendo en cuenta el contexto. 

Por tanto, la palabra banco ya no tiene la misma representación inicial sino que ya esta enriquezida por su contexto y la palabra banco tendrá representaciones distintas según el contexto lo cual es la clave y ayuda a diferenciarlo de lo embeddings. 

<mark>Ojo, acá los puntajes de similitud no se calculan con la similitud coseno sino que se calculan por medio del producto punto y la función softmax es la que los converte en pesos que suman 1.</mark>.

## Arquitectura transformer (sobre-simplificado)

Tokens $\rightarrow$ Embeddings+posición $\rightarrow$ Bloques de atención $\rightarrow$ Representación contextual.

Lo importante es entender el flujo: El texto entra como tokens, se convierte en vectores, pasa por capas de atención que lo contextualizan y sale una representación que entiende cada palabra en su contexto.

el paper de "attention is all you need (2017)" es la base de los grandes modelos de lenguaje como gpt. 

Esta formula de construcción del vector que representa la palabra se hace con TODAS  las palabras y se tiene en cuenta el orden de las palabras. 

"el perro mordió al hombre" $\neq$ "el hombre mordió al perro"

El contexto es distinto por el orden de las palabras. 

### Dos familias de modelos

#### Modelos Encoder-"Entender"
Lo que hace es recibir una secuencia completa  (leen toda la frase de una vez) y construyen representaciones contextuales usando información, trantan de entender para clasificar, buscar y analizar sentimientos. 

Ej: 
- clasificación de correos comos spam.
- Detectar entidades. 
- Detectar similitudes entre textos. 
- Búsqueda semántica. 

#### Modelos Decoder-"Generar"
Lo que es funcionar de una manera autorregresiva generando texto paso a paso.
Es un proceso muy pesado pero es como funcionan los modelos como GPT para generar texto. 


Ej: 
"El estudiante entregó ___" lo que hace el modelo es predecir, por ejemplo, Trabajo, tarea, exámen y luego sigue prediciendo diferentes palabras. Se van dando probabilidades según el contexto que hay atrás.  

algunos casos de uso: 
- Redactar correos. 
- Completas código. 
- Traducciones. 
- Generar explicaciones.
- Entre muchas otras cosas.



