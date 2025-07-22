# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01
La belleza de lo aleatorio.

### Actividad 02
El papel de la aleatoriedad en el código de Sofi está en permitir la creación de raíces y un "árbol" a partir de parámetros que combinan elementos aleatorios y no aleatorios. Es interesante el hecho de que el usuario pueda experimentar esa variabilidad de forma directa le aporta un valor extra, ya que convierte la obra en una experiencia generativa distinta para cada persona que la observa.

Dentro del campo de los videojuegos, uso la aleatoriedad en la generación procedural de niveles, como en los roguelites. Por ejemplo, en The Binding of Isaac, aunque uno elige un personaje con ciertas características, los enemigos, cofres y habitaciones se generan de forma aleatoria, haciendo que cada partida sea una experiencia única.

### Actividad 03
Realicé variantes en el movimiento del punto para que fuera más aleatorio y, además, se extendiera realizando saltos.
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
### Actividad 04

### Actividad 05
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
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x++;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
### Actividad 05
