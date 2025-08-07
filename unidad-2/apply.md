# Unidad 2: 🛠 Fase: Apply

## Actividad 08
### Describe el concepto de tu obra generativa. 
- *Es un movimiento de un circulo que va mediante una aceleracion de seno y cosen, mediante sufre un cambio de color distinguidos tambien mediante las ecuaciones del seno y coseno*

### ¿Cómo piensas aplicar el marco MOTION 101 y por qué?
- *Usando las formas de suma de los vectores en la velociadad con la posicion y la aceleracion con la velocidad*

#### ¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?
- *Seno y Coseno*

### El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.
- *Mediante las teclas de 1,2,3,4 se puede cambiar las tonalidades de los colores*

<details>
  <summary>Código</summary>

```js
let mover;
let colorMin = 100;
let colorMax = 255;

function setup() {
  createCanvas(600, 600);
  mover = new Mover();
  background(0);
}

function draw() {
  fill(220, 10); 
  rect(0, 0, width, height);

  mover.update();
  mover.show();
}

function keyPressed() {
  // Cambiar la escala de color según la tecla
  if (key === '1') {
    colorMin = 50;
    colorMax = 200;
  } else if (key === '2') {
    colorMin = 0;
    colorMax = 255;
  } else if (key === '3') {
    colorMin = 150;
    colorMax = 255;
  } else if (key === '4') {
    colorMin = 100;
    colorMax = 180;
  }
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.topspeed = 6;
    this.time = 0;
  }

  update() {
    let ax = sin(this.time) * 0.2;
    let ay = cos(this.time * 1.5) * 0.2;
    this.acceleration = createVector(ax, ay);

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);

    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1;
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }

    this.time += 0.05;
  }

  show() {
    noStroke();
    let c = color(
      map(sin(this.time), -1, 1, colorMin, colorMax),
      map(cos(this.time), -1, 1, colorMin, colorMax),
      255
    );
    fill(c);
    circle(this.position.x, this.position.y, 24);
  }
}
```
</details>

[Link del Sketch](https://editor.p5js.org/danipipe344/full/os6rP98Q9)

<img width="400" alt="image" src="https://github.com/user-attachments/assets/d857dce7-e59e-43d1-8579-95ec27c908ed" />
