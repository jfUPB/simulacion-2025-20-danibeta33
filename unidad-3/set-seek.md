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

### *Fuerza de resistencia a fluidos*

Explica cómo modelaste cada fuerza.
- *la variable growth representa la velocidad de cambio del radio, y puede ser modificada por fuerzas externas, y la resistencia a los fluidos que frena el movimiento*

Conceptualmente cómo se relaciona la fuerza con la obra generativa.
- *Se relaciona de tal forma en el movimiento aleatorio que tiene el objeto, mediante seste puede tomar un mayor recorrido o uno menor, dibujando asi algo en el canvas*

[Gotas](https://editor.p5js.org/danipipe344/full/_5IHrvl-H)

<details>
  <summary>Slimes</summary>
  
```js
let circles = [];
let lastSpawn = 0;
const spawnEvery = 1500;
let liquid;

function setup() {
  createCanvas(600, 400);
  noStroke();
  liquid = new Liquid(0, 0, width, height, 0.05);
}

function draw() {
  background(200, 30);

  if (millis() - lastSpawn > spawnEvery) {
    circles.push(new GrowingCircle(random(width), random(height)));
    lastSpawn = millis();
  }

  liquid.display();

  for (let c of circles) {
    if (liquid.contains(c)) {
      let drag = liquid.calculateDrag(c);
      c.applyForce(drag);
    }
    c.update();
    c.show();
  }
}

class GrowingCircle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.radius = 5;
    this.growth = createVector(0, random(2, 4));
    this.acc = createVector(0, 0);
    this.mass = random(0.5, 2);
    this.col = color(random(255), random(255), random(255), 120);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.growth.add(this.acc);
    this.radius += this.growth.y;
    this.acc.mult(0);
    this.radius = constrain(this.radius, 0, 200);
  }

  show() {
    fill(this.col);
    ellipse(this.pos.x, this.pos.y, this.radius * 2);
  }
}

class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  contains(circle) {
    return (circle.pos.x > this.x && circle.pos.x < this.x + this.w &&
            circle.pos.y > this.y && circle.pos.y < this.y + this.h);
  }

  calculateDrag(circle) {
    let speed = circle.growth.mag();
    let dragMag = this.c * speed * speed;
    let drag = circle.growth.copy();
    drag.mult(-1);
    drag.setMag(dragMag);
    return drag;
  }

  display() {
    fill(100, 150, 255, 50);
    rect(this.x, this.y, this.w, this.h);
  }
}
```
</details>

<img width="150" height="401" alt="image" src="https://github.com/user-attachments/assets/4fe8dcc1-d20e-41cc-9d63-40f512d2d486" />
