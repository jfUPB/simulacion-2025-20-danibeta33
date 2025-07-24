# Unidad 1
## 🤔 Fase: Reflect

### Actividad 9.1


### Describe la diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del Ruido Perlin (noise()). ¿En qué tipo de situación usarías cada una?

*El random funciona de que se elije un numero aleatorio entre un rango, con la cualidad de que todos tienen la misma posibilidad de salir, mientras que el noise del ruido Perlin, los datos elejidos aleatoriamente tienen un pequeña variacion o media segun el dato anteriormente elegido.*

*El random se usaria para casos donde se busca ejemplificar objetos totalmente aleatorias como lo seria un dado, y el noise lo usaria para tener datos que se busca lo aleatorio pero que tengan un tipo de parecido como una seleccion de paletas de colores, o texturas*

### Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?

*Es la capacidad de poder medir o limitar las elecciones de objetos, numeros o demas de forma aleatoria, donde se beneficie un punto o se requiera usar una media de ese numero*

*Una distriución uniforme, el walker caminara aleatoriamente dentro de un plano, sin ningun tipo de frecuencia favorecida. A diferencia de distribucion normal, donde hay unos datos con frecuencia favorecida, medida mediante una media y una desviación estandar*

### ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple

*1. La principal seria poder crear obras unicas donde la experiencia de usuario sea totalemente diferente para cada persona*

*2. La automatización de diferentes procesos, normalmente en el arte, el autor hace la obra, sin embargo con este metodo, el autor crea el sistema o el diseño para que este sea elavorada mediante codigo u otros sistemas*

### Piensa en tu obra final (Actividad 08). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas

*Requeria tener una forma de elegir de forma aleatoria los colores de los walkers para que pinten en el canvas, sin embargo que tambien no sean siempre iguales, pero se mantenga en la gama de color, por ello, la distribución Gausseana fue util para ese cado, donde se manipulo el R,G y B para que segun una paleta de colores original generara una paleta para cada walker.*

### ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?

*Un walker es una entidad que mediante una seleccion entre una escala de numeros, elije su movimiento de forma aleatoria dentro de un canvas, donde camina pixel a pixel por 4 u 8 casillas. Con Lévy flight el walker genera el mismo movimiento, pero con una particularidad, las posibilidades de moverse pixel a pixel pasa, pero tambien hay probabilidades de generar saltos por el canvas, y asi poder generar movimientos rapidos por todo el canvas*


### Actividad 9.2


#### ¿Cuál fue el concepto más abstracto o difícil de “visualizar” para ti en esta unidad? ¿Qué hiciste para finalmente comprenderlo?

*El mas abstracto o dificil de aplicar, en mi concepto es, el Lévy flight, debido a que dentro de un aspecto creativo, no encuentro un uso util o de forma consistente dentro de una generacion artistica, sin embargo entiendo que su aleatoridad evita que se mantenga constante, pero no le veo un aspecto creativo el cual se podria explotar*

#### Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.

*La modificacion de los parametros del background me permitieron bajar el alpha de la pintura, lo que genero que al modificarlo en muchas ocasiones pude geenrar un efecto de pintar, pero manteniendo resaltados los walkers, ademas moviendo esa paleta de colores, la ideal donde el color de los walker queda en el canvas, y no se intercambia con otros*
