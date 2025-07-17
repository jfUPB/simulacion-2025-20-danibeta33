# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01
La belleza de lo aleatorio

### Actividad 02
En la funcion Walker cambie el numero en el cual se divide del ancho y el alto del linzo, lo que esperaria que el punto iniciara su movimiento dentro de las respectivas proporciones del eje. Segun lo enseñado y lo practicado al ejecutarlo despues de cambiar se demostro que el punto inicia su movimiento mas a la izquierda superior, mediante mas grande sea hace el numero que se divide los parametros del canvas

``` js
    // The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2
    this.y = height / 2
  }

  show() {
    stroke('blue')
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(8));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } 
      else if (choice == 3){
      this.x += 5; 
    }
      else if (choice == 4){
      this.x -= 5;  
    } 
       else if (choice == 5){
      this.y += 5;  
    } 
    else if (choice == 6){
      this.y -= 5;  
    } 
      else {
      this.y--;
    }
  }
}

```
