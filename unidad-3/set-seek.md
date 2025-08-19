# Unidad 3: 🔎 Fase: Set + Seek

## Actividad 09
### *Fuerza de Fricción*

Explica cómo modelaste cada fuerza.
- *Modele la fuerza como una simple sumatoria de una fuerza de viento que hace que se mueva a la derecha mientras que lo contrasrreste una friccion que se elije de forma aleatoria, asi poder lograr tener una aceleracion 0, sin embargo esto no va a afectar a la obra*

Conceptualmente cómo se relaciona la fuerza con la obra generativa.
- *Se relaciona de tal forma en el movimiento aleatorio que tiene el objeto, mediante seste puede tomar un mayor recorrido o uno menor, dibujando asi algo en el canvas*

[Friccion](https://editor.p5js.org/danipipe344/full/KRhi39Zl1)

<details>
  <summary>Slimes</summary>
  
```js
let squares = [];
let lastSpawn = 0;
const spawnEvery = 2000;
let wind;

function setup() {
  createCanvas(600, 400);
  background(255);

  stroke(0, 50);
  noFill();
  for (let y = 20; y < height; y += 10) {
    beginShape();
    for (let x = 0; x < width; x += 10) {
      let offset = noise(x * 0.05, y * 0.05) * 20;
      vertex(x, y + offset);
    }
    endShape();
  }

  wind = createVector(0.05, 0);
  lastSpawn = -spawnEvery;
}

function draw() {
  if (millis() - lastSpawn >= spawnEvery) {
    squares.push(new FadingSquare());
    lastSpawn = millis();
  }

  for (let i = squares.length - 1; i >= 0; i--) {
    let s = squares[i];
    s.applyForce(wind);
    s.applyFriction();
    s.update();
    s.display();

    if (s.isDead()) {
      squares.splice(i, 1);
    }
  }
}

class FadingSquare {
  constructor() {
    this.size = 30;
    this.position = createVector(-this.size, random(this.size, height - this.size));
    this.velocity = createVector(2, 0); 
    this.acceleration = createVector(0, 0);
    this.birthTime = millis();
    this.mu = random(0.01, 0.05);
    let r = random(50, 255);
    let g = random(50, 255);
    let b = random(50, 255);
    this.startAlpha = random(50, 200);
    this.currentAlpha = this.startAlpha;
    this.color = color(r, g, b, this.startAlpha);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  applyFriction() {
    let friction = this.velocity.copy();
    if (friction.mag() > 0) {
      friction.normalize();
      friction.mult(-1);
      friction.mult(this.mu);
      this.applyForce(friction);
    }
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    let ageSeconds = floor((millis() - this.birthTime) / 1000);
    this.currentAlpha = this.startAlpha - ageSeconds * 40;
    this.color.setAlpha(max(this.currentAlpha, 0));
  }

  display() {
    noStroke();
    fill(this.color);
    rect(this.position.x, this.position.y, this.size, this.size, 4);
  }

  isDead() {
    return this.currentAlpha <= 0 || this.position.x > width;
  }
}

```
</details>

<img width="150" src="https://github.com/user-attachments/assets/1dd1907d-c6fb-436f-b260-e8ac23e48a5e" />
