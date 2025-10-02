# Evidencias de la unidad 6: Set💡

## Actividad 01
### Captura en tu bitácora dos imágenes de Tyler Hobbs que te llamen la atención y explica por qué.
> <img width="200" alt="image" src="https://github.com/user-attachments/assets/1ebe2a0a-5489-417c-86ce-95b8a5c1528b" />
>
> Me parece interesante como se pueden formar mediante lineas un sentido de profundidad a la obra.

> <img width="207" alt="image" src="https://github.com/user-attachments/assets/d71379b5-c299-42ad-be90-d82bbe80f207" />
>
> En esta obra simplemente me gusto el diseño que tomo de ramas o raices, quedo muy bien a mi punto de vista.

### ¿Qué te inspira de su trabajo?
>  Me aprece inspirador para proximos trabajos el hecho de poder generar trazos y que al chocar que se destruyan, para generar lineas no continuas o tambien la posibilidad de utilizar otras formas para representar el arte en vez de ser una sola linea
>

# Evidencias de la unidad 6: Seek-Investigación 🔎

## Actividad 02
### ¿Qué es una fuerza de dirección (steering force)?
> Es una fuerza qwue posee una direccion prevista, por ende puede moverse o desfasarse pero igual se trata de volver al camino destinado.
>

### ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?
> Las demas fuerzas tiene como un sentido universal, ya forman una fuerza que existe en la naturaleza, y las de direccion, como lo dice su nombre, poseen un factor de direccionalidad o destino previsto.
> 

### Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?
> Al ser como animales poseen un desitno y ademas un facotr de agrupacion, algo que se relaciona conceptualmente a la fuerza de direccion.
>

## Actividad 03
### Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
> Simplmenete se divide el canvas en partes donde se guardan los espacios en una matriz, y en cada espacio de ese canvas, se crea un vector normalizado que ira variando su direccion con el noise
>

### Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
> Este agente llega al espacio del canvas, lee el vector dentro de ese espacio, donde hace operaciones segun su destino y su velocidad para calcular el movmiento que va a realizar
>

### Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
> resolution, maxspeed, maxforce, número de vehículos, wraparound
>

### Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
> Cambie las lmitantes de la fuerza como la velocidad de los vehiculos para que fueran increiblemente rapido, ademas de quitar los angulos a los espacios de los canvas para que todos vayan a la misma direccion y en aunque no cambie nada del movimiento reduje de los espacios del canvas creando mas filas como columnas. Generando asi movmimientos rapidas asi una direccion con un poco de lag intencional.
>

> <img width="600" src="https://github.com/user-attachments/assets/250267d2-c609-47e0-8c2b-052a9443247c" />
>

## Actividad 04
