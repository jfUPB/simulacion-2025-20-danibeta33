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


