# Unidad 3: 🛠 Fase: Apply

## Explica cómo modelaste el problema de los n-cuerpos en tu obra.
- *Modele el problema de los n cuerpos usando cuadrados azules con masas generadas aleatoriamente con gaussiano, que se atraen entre si segun una fuerza gravitacional. Ademas, el mouse actua como un cuerpo central con gran masa, teniendo una fuerte atracción a todos los cuerpos*}

[Cuadrados](https://editor.p5js.org/danipipe344/full/RcYXX9gfs)

<details>
  <summary>Cuadrados</summary>
  
```js
let squares = [];
let numSquares = 10;
let G = 2;
let mouseStrength = 20;
let maxSpeed = 5;

function setup() {
  createCanvas(600, 600);
  noStroke();
  for (let i = 0; i < numSquares; i++) {
    squares.push(new BlueSquare());
  }
}

function draw() {
  background(20, 30, 60);

  for (let i = 0; i < squares.length; i++) {
    for (let j = i + 1; j < squares.length; j++) {
      let force = squares[i].attract(squares[j]);
      squares[i].applyForce(force);
      squares[j].applyForce(p5.Vector.mult(force, -1));
    }
  }

  let mousePos = createVector(mouseX, mouseY);
  for (let s of squares) {
    let dir = p5.Vector.sub(mousePos, s.pos);
    let d = constrain(dir.mag(), 5, 50); 
    dir.normalize();
    let strength = (mouseStrength * s.mass) / (d * d);
    dir.mult(strength);
    s.applyForce(dir);
  }

  for (let s of squares) {
    s.update();
    s.display();
  }
}

class BlueSquare {
  constructor() {
    this.reset();
    this.alpha = random(50, 200);
    this.fadeSpeed = random(0.5, 2);
    this.fadingOut = random([true, false]);
  }

  reset() {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D().mult(0.5);
    this.acc = createVector(0, 0);
    this.size = random(80, 200);
    
    this.mass = randomGaussian(50, 15); 
    this.mass = constrain(this.mass, 10, 100);
    
    this.col = color(random(0, 100), random(50, 150), random(150, 255));
  }

  applyForce(f) {
    let force = p5.Vector.div(f, this.mass);
    this.acc.add(force);
  }

  attract(other) {
    let dir = p5.Vector.sub(this.pos, other.pos);
    let d = constrain(dir.mag(), 10, 200);
    dir.normalize();
    let strength = (G * this.mass * other.mass) / (d * d);
    dir.mult(strength);
    return dir;
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);

    if (this.pos.x < 0 || this.pos.x > width) this.vel.x *= -1;
    if (this.pos.y < 0 || this.pos.y > height) this.vel.y *= -1;

    if (this.fadingOut) {
      this.alpha -= this.fadeSpeed;
      if (this.alpha <= 0) {
        this.reset();
        this.alpha = 0;
        this.fadingOut = false;
      }
    } else {
      this.alpha += this.fadeSpeed;
      if (this.alpha >= 255) {
        this.alpha = 255;
        this.fadingOut = true;
      }
    }
  }

  display() {
    fill(red(this.col), green(this.col), blue(this.col), this.alpha);
    rectMode(CENTER);
    rect(this.pos.x, this.pos.y, this.size, this.size);
  }
}

```
</details>

<img width="250" src="https://github.com/user-attachments/assets/cd0ef31a-0494-464f-9272-88981cb7c1c6" />
<img width="200" src="https://github.com/user-attachments/assets/9a62bab1-5db6-4f32-ae12-20a7d4c2c6f1" />
