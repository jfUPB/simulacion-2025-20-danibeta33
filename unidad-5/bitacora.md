# Evidencias de la unidad 5 - Seek: Investigación
## Actividad 02: ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?

### Ejemplo 4.2: an Array of Particles.
> En cada frame se generan nuevas particulas. Mediante un tiempo de visa, estas son recorridas al reves en un array para ser eliminadas. Asi gestionando la memoria y las particulas en pantalla.
>

### Ejemplo 4.4: a System of Systems.
> Ocurre lo mismo que lo anterior, la unica diferencia es que el concepto es un que sucede dentro de sistemas en otros sistemas.
>

### Ejemplo 4.5: a Particle System with Inheritance and Polymorphism.
> El sistema es el mismo, la diferencia viene siendo de que forma aleatoria se elijen entre un cuadrado o un circulo, generando particulas de un polimorfismo, pero la eliminacion y administracion se mantiene igual a los otros ejemplos.
>

### Ejemplo 4.6: a Particle System with Forces.
> Cada partícula sigue el mismo ciclo de vida, pero ahora acumula y aplica fuerzas. Al morir y ser eliminada, también se liberan esos objetos internos
>

### Ejemplo 4.7: a Particle System with a Repeller.
> El repulsor añade una fuerza localizada pero la gestión de partículas es idéntica: creación con new, desaparición con isDead() y eliminación del array.
>

## Actividad 02: Experimentos
### Ejemplo 4.2: an Array of Particles.
<details>
  <summary>Codigo Ejemplo 4.2: Particle</summary>
  
```js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 400.0;

    this.noiseOffset = random(10); 
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    colorMode(HSB, 255); 
    stroke(0, this.lifespan);
    strokeWeight(2);
    
    let n = noise(this.noiseOffset + (255 - this.lifespan) * 0.02);
    let hue = map(n, 0, 1, 0, 255);

    fill(hue, 200, 255, this.lifespan);
    circle(this.position.x, this.position.y, 8);

    colorMode(RGB, 255); 
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.2: Sketch</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let particles = [];

function setup() {
  createCanvas(700, 900);
}

function draw() {
  background(255);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      //remove the particle
      particles.splice(i, 1);
    }
  }
}
```
</details>


[Enlace Ejemplo 4.2](https://editor.p5js.org/danipipe344/full/ToGpPGcIy)

<img width="300" src="https://github.com/user-attachments/assets/15630ac2-19b9-4e8b-a7f7-5c9f46465e9f" />
<br></br>

> El ejercicio ya mantenia el sistema de eliminacion de particulas, esta se maneja on un timepo de vida, donde se revisa un array dee las particulas, y las borra en el orden contrario. **Los cambios** que hice fue poner un noise, debido a la sencilles del ejercicio me parecio correcto simplemente cambiar los colores de la particula mientras viven, ademas para poder visualizarlo bien agrande el canvas y alargue la vida de las particulas.
>


### Ejemplo 4.4: a System of Systems.

<details>
  <summary>Codigo Ejemplo 4.4: Sketch</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Particles are generated each cycle through draw(),
// fall with gravity and fade out over time
// A ParticleSystem object manages a variable size
// list of particles.

// an array of ParticleSystems
let emitters = [];

function setup() {
  createCanvas(640, 240);
  let text = createP("click to add particle systems");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.4: Particle</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    
    let vx = randomGaussian(0, 1); 
    let vy = randomGaussian(-1, 0.5);
    
    this.velocity = createVector(vx, vy);
    
    this.lifespan = 255.0;
    this.size = constrain(randomGaussian(8, 2), 4, 12);
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, this.size);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}

```
</details>

<details>
  <summary>Codigo Ejemplo 4.4: Emitter</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    // Looping through backwards to delete
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        // Remove the particle
        this.particles.splice(i, 1);
      }
    }

    // Run every particle
    // ES6 for..of loop
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
    // https://www.youtube.com/watch?v=Y8sMnRQYr3c
    // for (let particle of this.particles) {
    //   particle.run();
    // }

    // Filter removes any elements of the array that do not pass the test
    // I am also using ES6 arrow snytax
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions
    // https://www.youtube.com/watch?v=mrYMzpbFz18
    // this.particles = this.particles.filter(particle => !particle.isDead());

    // Without ES6 arrow code would look like:
    // this.particles = this.particles.filter(function(particle) {
    //   return !particle.isDead();
    // });
  }
}

```
</details>

[Enlace Ejemplo 4.4](https://editor.p5js.org/danipipe344/full/PWvI6aHJl)

<img width="300" height="230" alt="image" src="https://github.com/user-attachments/assets/4297a61f-d75d-46ca-850c-630776878259" />
<br></br>

> El ejercicio ya mantenia el sistema de eliminacion de particulas, esta se maneja on un tiempo de vida, donde se revisa un array dee las particulas, y las borra en el orden contrario, ademas de mantener por medio de una funcion crear emisores, y revisar esos emisores con sus propias funciones. **Los cambios** que hice fue poner una random gaussiano para la velocidad y el tamaño, asi poder ver cambios en la geenracion de particulas, manteniendo la simplesa del ejercicio
>

### Ejemplo 4.5: a Particle System with Inheritance and Polymorphism.

<details>
  <summary>Codigo Ejemplo 4.5: Sketch</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Particles are generated each cycle through draw(),
// fall with gravity and fade out over time
// A ParticleSystem object manages a variable size
// list of particles.


let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
}

function draw() {
  background(255);
  emitter.addParticle();
  emitter.run();
}
```
</details>
<details>
  <summary>Codigo Ejemplo 4.5: Emitter</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

 
levy(min = 0.01, max = 1) {
    let beta = 1.5; 
    let r = random(min, max);
    return pow(1 / r, 1 / beta);
  }

  addParticle() {
   
    let step = this.levy();
    let chance = random(1);

    let probConfetti = 0.1 + 0.02 * step; // base = 10%
   if (chance < probConfetti) {
  this.particles.push(new Confetti(this.origin.x, this.origin.y));
  } else {
  this.particles.push(new Particle(this.origin.x, this.origin.y));
  }
  }


  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.5: Partcile</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.5: Confeti</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Child class constructor
class Confetti extends Particle {
  // Override the show method
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);

    rectMode(CENTER);
    fill(127, this.lifespan);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}
```
</details>

[Enlace Ejemplo 4.5](https://editor.p5js.org/danipipe344/full/yhVpWwkrA)

<img width="300" src="https://github.com/user-attachments/assets/3a35a136-a1f7-4fb1-83eb-814a4d649460" />
<br></br>

> El ejercicio ya mantenia el sistema de eliminacion de particulas, esta se maneja on un tiempo de vida, donde se revisa un array de las particulas, y las borra en el orden contrario, ademas de mantener por medio de una funcion crear emisores, y revisar esos emisores con sus propias funciones. **Los cambios** fue añadir un salto de levy en la generacion de la particula de Confetti, teniendo un porcentaje menor de aparicion en vez de ser por algo aleatorio.
>

### Ejemplo 4.6: a Particle System with Forces.

<details>
  <summary>Codigo Ejemplo 4.6: Sketch</summary>
  
```js
let emitter;

function setup() {
  createCanvas(1280, 480);
  emitter = new Emitter(width / 4, 50);
}

function draw() {
  background(255, 30);
 let gravity = createVector(0, 0.1);
  
  emitter.addParticle();
  emitter.applyForce(gravity);

  
  if (mouseIsPressed) {
    let wind = createVector(0.2, 0); 
    emitter.applyForce(wind);
  }

  emitter.run();
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.6: Particle</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0.0);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.lifespan = 255.0;
    this.mass = 1; // Let's do something better here!
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2.0;
  
    
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.6: Emitter</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Using a for of loop to apply the force to all particles
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }
   

  run() {
    //{!7} Can’t use the enhanced loop because checking for particles to delete.
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
  
   
}
```
</details>

[Enlace Ejemplo 4.6](https://editor.p5js.org/danipipe344/full/QbzbcmbEv)

<img width="300" src="https://github.com/user-attachments/assets/0ffbb4d6-a43c-4b68-a871-dd80e3dcb2e7" />
<br></br>

> El ejercicio ya mantenia el sistema de eliminacion de particulas, esta se maneja on un tiempo de vida, donde se revisa un array de las particulas, y las borra en el orden contrario, ademas de mantener por medio de una funcion crear emisores, y revisar esos emisores con sus propias funciones. Poseia una logica para asignar las fuerzas a las particulas mediante el emisor **Los cambios** que hice fue implementar segunla logica que ya tenia de las fuerzas ponerle un viento medianamente fuerte a las particulas cuando se le da click al canvas.
>

### Ejemplo 4.7: a Particle System with a Repeller.

<details>
  <summary>Codigo Ejemplo 4.7: Sketch</summary>
  
```js
// One ParticleSystem
let emitter;

//{!1} One repeller
let repeller;
let attractor;

let G = 0.5;

function setup() {
  createCanvas(640 , 240);
   emitter = new Emitter(width / 2, 60);
   repeller = new Repeller(width / 4, height - 20);
   attractor = new Attractor(width / 1.2, height -20);
}

function draw() {
  background(255);
  emitter.addParticle();
  // We’re applying a universal gravity.
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);
  //{!1} Applying the repeller
  emitter.applyRepeller(repeller);
  emitter.run();

  repeller.show();
  attractor.show();;
  
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.7: Attractor</summary>
  
```js
class Attractor {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.mass = 20;
  }

  attract(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 25);
    let strength = (G * this.mass * 1) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  show() {
    stroke(0);
    fill(100, 150, 255, 150);
    circle(this.position.x, this.position.y, this.mass * 2);
  }
}

```
</details>

<details>
  <summary>Codigo Ejemplo 4.7: Emitter</summary>
  
```js
//{!1} The Emitter manages all the particles.
class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Applying a force as a p5.Vector
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  applyRepeller(repeller) {
    //{!4} Calculating a force for each Particle based on a Repeller
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }
  
   applyAttractor(attractor) {
    for (let particle of this.particles) {
      let force = attractor.attract(particle);
      particle.applyForce(force);
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.7: Particle</summary>
  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(random(1, 3));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}
```
</details>

<details>
  <summary>Codigo Ejemplo 4.7: Repeller</summary>
  
```js
class Repeller {
  constructor(x, y) {
    this.position = createVector(x, y);
    //{!1} How strong is the repeller?
    this.power = 150;
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, 32);
  }

  repel(particle) {
    //{!6 .code-wide} This is the same repel algorithm we used in Chapter 2: forces based on gravitational attraction.
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);
    let strength = (-1 * this.power) / (distance * distance);
    force.setMag(strength);
    return force;
  }
}
```
</details>

[Enlace Ejemplo 4.7](https://editor.p5js.org/danipipe344/full/a7_sspM_E)

<img width="300" src="https://github.com/user-attachments/assets/10513fd3-5f0e-4785-bfa0-1eaf99fd9711" />
<br></br>

> El ejercicio ya mantenia el sistema de eliminacion de particulas, esta se maneja on un tiempo de vida, donde se revisa un array de las particulas, y las borra en el orden contrario, ademas de mantener por medio de una funcion crear emisores, y revisar esos emisores con sus propias funciones. Poseia una logica para asignar las fuerzas a las particulas mediante el emisor **Los cambios** que hice fue, crear una archivo attractor, usando de referencia el ejemplo de *https://editor.p5js.org/natureofcode/sketches/Cl0Eeaz_V*, donde lo implemente en el codigo, mediante la misma forma que el objeto que repele a las particulas
>

# Evidencias de la unidad 5 - Apply: Aplicación
## Concepto
### Boceto

<img width="300" src="https://github.com/user-attachments/assets/55328859-7e0b-4d97-9eb3-f3edb65671c9" />
<br></br>

### ¿Cuál es el concepto de tu obra? ¿Qué quieres comunicar con ella?
>El concepto nacio de tomar primero las ideas que queria de cada unidad. Tome las formaciones de seno y coseno, formando asi un tipo de ADN, pero me gusto el hecho de mezlcarlo con lago espacil como el cosmos, asi que lo plantee el canvas como si fuera un cielo estrellado, y el atractor una estrella gigante que atrae a las pequeñas particulas en primer plano
>

## Obra: Atracción entre polos 

### Polimorfismo y Herencia en el proyecto
>Ya que son 3 tipos de particuals que se manifiestan en el proyecto, cree una base, de ahi cree clases hijas que son: Azul y Estrella. DOnde solo utilizan la logica o la forma basica de la particula y unicamente van a modificar las cosas como el uptade o el show, y en casos el isDead.
>

### Referencia de otras unidades
>Use el ruido de Perlin de la unidad 1 para el cambio de color de las particulas al acercarse al objeto atractor; De la unidad 2, se uso el motio 101; Unidad 3 el diseño y logica de una fuerza de atraccion; De la Unidad 4 las ondas senos y cosenos para el movimiento de las particulas. 
>

### Gestion del tiempo de vida de las partículas y la memoria
>Se usaron los ejemplos de la unidad, simplificando con ayuda de la IA, pero la logica se mantiene, un bucle donde se maneja un array eliminando las particulas en sentido inverso mientras acaban su tiempo de vida. LAs particulas base como las azules se manejan mediante el emisor como lo hacen en los ejemplos de la unidad. Y las estrellas funcionan con la misma logica de vida y muerte pero estas se manejan en el sketch debido al fujo y las diferencias de creacion.
>

[Enlace Atraccion entre polos](https://editor.p5js.org/danipipe344/full/slEp8Kvtb)


<details>
  <summary>Codigo Atraccion entre polos: Sketch</summary>
  
```js
let emitter;
let attractor;
let stars = [];

function setup() {
  createCanvas(640, 480);

  for (let i = 0; i < 80; i++) {
    stars.push(new ParticleStar(random(width), random(height)));
  }

  background(10, 15, 40);
  emitter = new Emitter(width / 2, 0);
  attractor = new Attractor();
}

function draw() {
  noStroke();
  fill(10, 15, 40, 50);
  rect(0, 0, width, height);

  for (let i = stars.length - 1; i >= 0; i--) {
    let s = stars[i];
    s.update();
    s.show();
    if (s.isDead()) {
      stars.splice(i, 1);
    }
  }

  if (random(1) < 0.02) { // 2% probabilidad por frame
    stars.push(new ParticleStar(random(width), random(height)));
  }

  emitter.addParticle();
  emitter.applyAttractor(attractor);
  emitter.run(attractor);

  // Dibujar atractor
  attractor.show();
}
```
</details>

<details>
  <summary>Codigo Atraccion entre polos: Attractor</summary>
  
```js
class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.G = 5;
  }

  attract(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 20, 100); 
    let strength = (this.G * this.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  show() {
    this.position.set(mouseX, mouseY);

    noStroke();
    fill(255, 180);
    circle(this.position.x, this.position.y, this.mass * 2);

    
    noFill();
    stroke(255, 80);
    strokeWeight(4);
    circle(this.position.x, this.position.y, this.mass * 3);

    stroke(255, 40);
    circle(this.position.x, this.position.y, this.mass * 5);
  }
}
```
</details>

<details>
  <summary>Codigo Atraccion entre polos: Emitter</summary>
  
```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let type = random(1) < 0.5 ? "red" : "blue";
    if (type === "red") {
      this.particles.push(new ParticleBase(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new ParticleBlue(this.origin.x, this.origin.y));
    }
  }

  applyAttractor(attractor) {
    for (let p of this.particles) {
      let force = attractor.attract(p);
      p.applyForce(force);
    }
  }

  run(attractor) {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.show(attractor);
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```
</details>
<details>
  <summary>Codigo Atraccion entre polos: ParticleBase</summary>
  
```js
class ParticleBase {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 2);
    this.acceleration = createVector(0, 0);

    this.angle = random(TWO_PI);
    this.amplitude = 80;
    this.angularVelocity = 0.1;

    this.lifespan = 500;

    this.noiseOffset = random(1000); 
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);

    this.angle += this.angularVelocity;
    this.offsetX = this.amplitude * sin(this.angle);

    this.lifespan -= 2.0;
  }

  show(attractor) {
    push();
    strokeWeight(2);

    // Contorno fijo rojo
    stroke(255, 0, 0, this.lifespan);

    let d = p5.Vector.dist(this.position, attractor.position);
    let colR, colG, colB;

    // Mayor radio de influencia
    let influenceRadius = attractor.mass * 8;

    if (d < influenceRadius) {
      let factor = map(d, 0, influenceRadius, 1, 0);
      let n = noise(this.noiseOffset + frameCount * 0.02);

      colR = lerp(200, 255, n * factor);
      colG = lerp(50, 150, n * factor);
      colB = lerp(50, 100, n * factor);
    } else {
      colR = 255;
      colG = 255;
      colB = 255;
    }

    fill(colR, colG, colB, this.lifespan);
    circle(this.position.x + this.offsetX, this.position.y, 8);
    pop();
  }

  isDead() {
    return (this.lifespan < 0 || this.position.y > height);
  }
}
```
</details>
<details>
  <summary>Codigo Atraccion entre polos: ParticleBlue</summary>
  
```js
class ParticleBlue extends ParticleBase {
  constructor(x, y) {
    super(x, y);
  }

  show(attractor) {
    push();
    strokeWeight(2);

    // Contorno fijo azul
    stroke(0, 0, 255, this.lifespan);

    let d = p5.Vector.dist(this.position, attractor.position);
    let colR, colG, colB;

    let influenceRadius = attractor.mass * 8;

    if (d < influenceRadius) {
      let factor = map(d, 0, influenceRadius, 1, 0);
      let n = noise(this.noiseOffset + frameCount * 0.02);

      colR = lerp(50, 120, n * factor);
      colG = lerp(50, 200, n * factor);
      colB = lerp(200, 255, n * factor);
    } else {
      colR = 255;
      colG = 255;
      colB = 255;
    }

    fill(colR, colG, colB, this.lifespan);
    circle(this.position.x + this.offsetX, this.position.y, 8);
    pop();
  }
}
```
</details>
<details>
  <summary>Codigo Atraccion entre polos: ParticleStar</summary>
  
```js
class ParticleStar extends ParticleBase {
  constructor(x, y) {
    super(x, y);
    this.lifespan = random(200, 500); 
    this.baseX = x;
    this.baseY = y;
    this.noiseOffsetX = random(1000);
    this.noiseOffsetY = random(2000);
  }

  update() {
    // Movimiento suave con ruido perlin
    this.position.x = this.baseX + map(noise(this.noiseOffsetX + frameCount * 0.002), 0, 1, -2, 2);
    this.position.y = this.baseY + map(noise(this.noiseOffsetY + frameCount * 0.002), 0, 1, -2, 2);

    // Decaimiento de vida
    this.lifespan -= 0.3;
  }

  show() {
    push();
    noStroke();
    fill(255, this.lifespan); 
    circle(this.position.x, this.position.y, 2);
    pop();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}
```
</details>

<img width="400" src="https://github.com/user-attachments/assets/ee667aac-0285-4958-9f07-914ce3629083" />
<br></br>

