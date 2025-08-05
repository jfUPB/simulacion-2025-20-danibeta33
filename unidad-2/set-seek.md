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
