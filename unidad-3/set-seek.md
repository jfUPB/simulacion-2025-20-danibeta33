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
const c = 0.05; 

function setup() {
  createCanvas(600, 400);
  noStroke();
}

function draw() {
  background(200, 30);

  if (millis() - lastSpawn > spawnEvery) {
    circles.push(new GrowingCircle(random(width), random(height)));
    lastSpawn = millis();
  }

  for (let c of circles) {
    let drag = calculateDrag(c);
    c.applyForce(drag);
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

function calculateDrag(circle) {
  let speed = circle.growth.mag();
  let dragMag = c * speed * speed;
  let drag = circle.growth.copy();
  drag.mult(-1);
  drag.setMag(dragMag);
  return drag;
}
```
</details>

<img width="150" height="401" alt="image" src="https://github.com/user-attachments/assets/4fe8dcc1-d20e-41cc-9d63-40f512d2d486" />

### *Fuerza de atracción gravitacional*

Explica cómo modelaste cada fuerza.
- *Simplemente se modelo un circulo en el centro del canvas que genera una fuerza de atraccion, afectando a cada mover individualmente*

Conceptualmente cómo se relaciona la fuerza con la obra generativa.
- *Los movers con sus colores dejan un rastro movimndose un de forma aleatoria mientas son atraidas hacia el centro*

[Gravitación](https://editor.p5js.org/danipipe344/full/sidSwYScf1)

<details>
  <summary>Slimes</summary>
  
```js
let attractor;
let movers = [];
let lastSpawn = 0;
const spawnEvery = 1200;
const G = 1;

function setup() {
  createCanvas(600, 400);
  attractor = new Attractor(width / 2, height / 2, 20);
}

function draw() {
  background(90, 10);

  if (millis() - lastSpawn > spawnEvery) {
    movers.push(new Mover(random(width), random(height), random(0.5, 2)));
    lastSpawn = millis();
  }

  for (let m of movers) {
    let force = attractor.attract(m);
    m.applyForce(force);
    m.update();
    m.display();
  }

  attractor.display();
}

class Mover {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-1, 1), random(-1, 1));
    this.acc = createVector(0, 0);
    this.mass = m;
    this.col = color(random(100, 255), random(100, 255), random(100, 255), 180);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(5);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display() {
    noStroke();
    fill(this.col);
    ellipse(this.pos.x, this.pos.y, this.mass * 16);
  }
}

class Attractor {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.mass = m;
  }

  attract(mover) {
    let force = p5.Vector.sub(this.pos, mover.pos);
    let distance = force.mag();
    distance = constrain(distance, 5, 25);
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  display() {
    noStroke();
    fill(200, 0, 100, 200);
    ellipse(this.pos.x, this.pos.y, this.mass * 2);
  }
}
```
</details>

<img width="150" alt="image" src="https://github.com/user-attachments/assets/cfcdbff3-35d9-4f3e-8a71-084da2482f6f" />
