# Unidad 2: 🔎 Fase: Set + Seek

## Actividad 01
¿Cómo funciona la suma dos vectores en p5.js?
- *Funcionan con el add. debido a que al ser vectores deben sumarse mediante componente a componente*

¿Por qué esta línea position = position + velocity; no funciona?
- *Debido a que al ser vectores, no objetos, se requieren sumar por componentes*

## Actividad 02
¿Qué tuviste que hacer para hacer la conversión propuesta?
- *Cambiar la forma de guardado de la posicion, en un vector*

<details>
  <summary>Código</summary>
  
```js

let walker;


function setup() {
  createCanvas(640, 240);
  background(255);
  walker = new Walker(width/2,height/2);
  
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor(_x,_y) {
    
    this.position = createVector(_x,_y);  
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}
```
</details>

## Actividad 03
¿Qué resultado esperas obtener en el programa anterior?
- *Simplemente que se escribiera por consola los mensajes del Log*

¿Qué resultado obtuviste?
- *Se obtuvieron los resultados previstos, en el draw no se involucro nada, asi q solo se hace un canvas*

Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
- *Paso por valor*

<details>
  <summary>Código</summary>
  
```js
  let num = 1;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}

function cambiar(num2) {
  num2 = 100;
}

cambiar(num);
console.log(num);
```
</details>

- *Paso por referencia*
  
<details>
  <summary>Código</summary>
  
```js
 let numero = {num:1};

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}

function cambiar(num2) {
  num2.numero = 100;
}

cambiar(numero);
console.log(numero);
```
</details>

¿Qué tipo de paso se está realizando en el código?
- *Se gace por referencia, mas especificamente en el playinVector, donde entra al position y cambia referenciado a X y Y*
  
¿Qué aprendiste?
- *El uso de referencia y objeto, su diferencia al canipular la informacion, como se guarada en sistemas el valor por valor, y las diferencias de uso*

## Activdad 04
¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
- *El metodo mag() da la magnitud del vector exacto, a diferencia del magSq(), este se ahorra sacar raiz cuadrada. Por ello, tal vez para procedimientos grandes o constantes el mas eficiente es magSq() debido a que se ahorra el procedimiento de la raiz*

¿Para qué sirve el método normalize()?
- *Sirve para sacar el vecotr unitario del vector*

Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
- *Nos sirve para saber la relacion de direccion entre dos vectores*

El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
- *La forma de llamar al metodo*

Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
- *El resultado de un producto cruz es un vector que se encuentra perpendicular al plano que forman los vectores que se usaron para la multiplicacion. Un ejemplo claro es es el producto cruz del eje X y el eje Y es el eje Z*

¿Para que te puede servir el método dist()?
- *Este metodo sirve para saber la distancia entre dos vectores, un uso es que su porpia operacion es el calculo de una magnitud de otro vector, por lo que nos daria el valor de un supuesto vector 3 que conecta a los vectores 1 y 2*

¿Para qué sirven los métodos normalize() y limit()?
- *El metodo normalice() nos da el vecotr unitario y el limit() limita la magnitud de un vector*

## Actividad 05
