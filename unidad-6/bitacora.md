# Evidencias de la unidad 6: Set💡

## Actividad 01
### Captura en tu bitácora dos imágenes de Tyler Hobbs que te llamen la atención y explica por qué.
> <img width="200" alt="image" src="https://github.com/user-attachments/assets/1ebe2a0a-5489-417c-86ce-95b8a5c1528b" />
>
> Me parece interesante como se pueden formar mediante lineas un sentido de profundidad a la obra.

> <img width="207" alt="image" src="https://github.com/user-attachments/assets/d71379b5-c299-42ad-be90-d82bbe80f207" />
>
> En esta obra simplemente me gusto el diseño que tomo de ramas o raices, quedo muy bien a mi punto de vista.

### ¿Qué te inspira de su trabajo?
>  Me aprece inspirador para proximos trabajos el hecho de poder generar trazos y que al chocar que se destruyan, para generar lineas no continuas o tambien la posibilidad de utilizar otras formas para representar el arte en vez de ser una sola linea
>

# Evidencias de la unidad 6: Seek-Investigación 🔎

## Actividad 02
### ¿Qué es una fuerza de dirección (steering force)?
> Es una fuerza qwue posee una direccion prevista, por ende puede moverse o desfasarse pero igual se trata de volver al camino destinado.
>

### ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?
> Las demas fuerzas tiene como un sentido universal, ya forman una fuerza que existe en la naturaleza, y las de direccion, como lo dice su nombre, poseen un factor de direccionalidad o destino previsto.
> 

### Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?
> Al ser como animales poseen un desitno y ademas un facotr de agrupacion, algo que se relaciona conceptualmente a la fuerza de direccion.
>

## Actividad 03
### Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
> Simplmenete se divide el canvas en partes donde se guardan los espacios en una matriz, y en cada espacio de ese canvas, se crea un vector normalizado que ira variando su direccion con el noise
>

### Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
> Este agente llega al espacio del canvas, lee el vector dentro de ese espacio, donde hace operaciones segun su destino y su velocidad para calcular el movmiento que va a realizar
>

### Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
> resolution, maxspeed, maxforce, número de vehículos, wraparound
>

### Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
> Cambie las lmitantes de la fuerza como la velocidad de los vehiculos para que fueran increiblemente rapido, ademas de quitar los angulos a los espacios de los canvas para que todos vayan a la misma direccion y en aunque no cambie nada del movimiento reduje de los espacios del canvas creando mas filas como columnas. Generando asi movmimientos rapidas asi una direccion con un poco de lag intencional.
>

> <img width="600" src="https://github.com/user-attachments/assets/250267d2-c609-47e0-8c2b-052a9443247c" />
>

## Actividad 04
### Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
>Cada vehiculo posee un area de calculo y ubica a los otros vehiculos con su respectivo destino, velcoidades, para poder calcular asi un promedio de las velocidades y su destino, ademas de poder generar un espacio mediante un tipo de fuerzas que lo repelen entre los otros vehiculos.
>

### Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
>neighborDistance, desiredSeparation, maxspeed, maxforce
>

### Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
> Modifique sus relgas del flocking para que puedan pegarse mas creando partes donde se dispersan y luego se concentran, todos yendo a un camino. Ademas de dar una fuerza con el mouse, para hacer como si fuerta un enjambre guiado. 
>

> <img width="600" src="https://github.com/user-attachments/assets/5729f541-2069-407c-be2b-1989d58c94d7" />
>

# Evidencias de la unidad 6: Apply-Aplicación 🛠

## Actividad 05
### Documenta todo el proceso de diseño y creación en tu bitácora, incluyendo bocetos y decisiones de diseño.
><img width="300" src="https://github.com/user-attachments/assets/22d46564-f56f-449d-be57-429ff5a674cb" />
>

### El código fuente completo de tu sketch en p5.js.

<details>
  <summary>flowfield.js</summary>
  
```js
// FlowField: matriz 2D de vectores (influencia ligera en las partículas)
class FlowField {
  constructor(resolution = 40) {
    this.resolution = resolution;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = new Array(this.cols * this.rows);
    this.zoff = 0;
    this.update(); // inicializa
  }

  update() {
    let xoffBase = random(1000) * 0; // opcional seed
    let yoff = 0;
    for (let j = 0; j < this.rows; j++) {
      let xoff = 0;
      for (let i = 0; i < this.cols; i++) {
        // Perlin noise para ángulo; factor 2 para un poco más de giro local
        let angle = noise(xoff, yoff, this.zoff) * TWO_PI * 2;
        this.field[i + j * this.cols] = p5.Vector.fromAngle(angle);
        xoff += 0.12;
      }
      yoff += 0.12;
    }
    // cambia muy lentamente en el tiempo para que el campo evolucione
    this.zoff += 0.002;
  }

  // Devuelve copia del vector del campo para la posición dada
  lookup(pos) {
    let col = constrain(floor(pos.x / this.resolution), 0, this.cols - 1);
    let row = constrain(floor(pos.y / this.resolution), 0, this.rows - 1);
    return this.field[col + row * this.cols].copy();
  }
}

```
</details>

<details>
  <summary>particle.js</summary>
  
```js
class Particle {
  constructor(pos, vel, col) {
    this.pos = pos.copy();
    this.prev = this.pos.copy(); // guardar posición anterior para dibujar línea
    this.vel = vel.copy();
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.color = col;
  }

  applyFlow(flow) {
    let force = flow.lookup(this.pos);
    this.applyForce(force.mult(0.05));
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.prev = this.pos.copy(); // antes de movernos
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 4; // mueren un poco más rápido
  }

  show() {
    stroke(this.color);
    strokeWeight(5);
    line(this.prev.x, this.prev.y, this.pos.x, this.pos.y); // línea en vez de punto
  }

  isDead() {
    return this.lifespan <= 0;
  }
}
```
</details>

<details>
  <summary>sketch.js</summary>
  
```js
let flow;
let particles = [];
let triBg;

// audio
let song;
let fft;

// colores
const highColors = ["#fbdc03", "#b9d6aa", "#e16529"];
const lowColors  = ["#64705c", "#a31e21"];

// umbrales dinámicos para percusión
let bassHistory = [];
const historySize = 60; // ~1 segundo de historial (si quieres usarlo luego)
let beatHoldFrames = 20; 
let beatCutoff = 0;
let beatDecayRate = 0.98;
let framesSinceLastBeat = 0;

function preload() {
  song = loadSound("Los Caligaris - Kilómetros (video oficial).mp3");
}

function setup() {
  createCanvas(750, 750);
  background("#e1dbbb");

  flow = new FlowField(40);

  triBg = new TriBackground({
    numRays: 12,
    thickness: 210,  // grosor mayor según lo pediste
    lengthFactor: 1.1,
    color: "#913f41",
    rotationSpeed: 0.005
  });

  fft = new p5.FFT(0.9, 1024);
  fft.setInput(song);

  let playBtn = createButton("Play / Pause");
  playBtn.position(10, 10);
  playBtn.mousePressed(togglePlay);
}

function draw() {
  background("#e1dbbb");

  triBg.draw();
  triBg.update();
  flow.update();

  if (song && song.isPlaying()) {
    let spectrum = fft.analyze(); 
    let bass = fft.getEnergy("bass"); 
    let treble = fft.getEnergy("treble");
    let mid = fft.getEnergy("mid");

    detectBeat(bass); // dispara con el golpe de batería (bass)

    // si quieres también explosiones por agudos/medios mantenlas:
    if (treble > 140 || mid > 140) {
      // pequeña probabilidad para no sobrecargar; opcional:
      if (random() < 0.08) spawnExplosion("high");
    }
  }

  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.applyFlow(flow);
    p.update();
    p.show();
    if (p.isDead()) particles.splice(i, 1);
  }
}

function detectBeat(bass) {
  // lógica de detección de pico en graves (kick)
  if (bass > beatCutoff && bass > 150) {
    spawnExplosion("low");
    beatCutoff = bass * 1.1;
    framesSinceLastBeat = 0;
  } else {
    if (framesSinceLastBeat <= beatHoldFrames) {
      framesSinceLastBeat++;
    } else {
      beatCutoff *= beatDecayRate;
      beatCutoff = max(beatCutoff, 150);
    }
  }
}

function spawnExplosion(type) {
  // elegir un único color de la paleta (por explosión)
  let colArray = type === "high" ? highColors : lowColors;
  let chosenColor = random(colArray);

  let origin = createVector(
    random(width * 0.3, width * 0.7), 
    random(height * 0.3, height * 0.7)
  );

  let numParticles = 80; // moderado para no saturar
  for (let i = 0; i < numParticles; i++) {
    let angle = random(TWO_PI);
    let speed = random(2, 6);
    let vel = p5.Vector.fromAngle(angle).mult(speed);
    particles.push(new Particle(origin.copy(), vel, chosenColor));
  }

  // límite máximo de partículas vivas en pantalla
  if (particles.length > 600) {
    particles.splice(0, particles.length - 600);
  }
}

// CORRECCIÓN: definimos togglePlay aquí para evitar ReferenceError
function togglePlay() {
  if (!song) return;
  if (song.isPlaying()) {
    song.pause();
  } else {
    song.play();
  }
}

function keyPressed() {
  if (key === " ") togglePlay();
}
```
</details>

<details>
  <summary>triBackground.js</summary>
  
```js
// TriBackground: dibuja triángulos gruesos radiales rotando con velocidad constante.
class TriBackground {
  constructor(opts = {}) {
    this.numRays = opts.numRays ?? 12;
    this.thickness = opts.thickness ?? 500; // más gruesos por defecto
    this.lengthFactor = opts.lengthFactor ?? 0.95;
    this.color = opts.color ?? "#913f41";
    this.rotationSpeed = opts.rotationSpeed ?? 0.01;
    this.angleOffset = 0;
  }

  update(dt = 1) {
    this.angleOffset += this.rotationSpeed * dt;
  }

  draw() {
    push();
    translate(width / 2, height / 2);
    fill(this.color);
    noStroke();
    const L = width * this.lengthFactor;
    const halfT = this.thickness / 2;
    for (let i = 0; i < this.numRays; i++) {
      let angle = this.angleOffset + (TWO_PI / this.numRays) * i;
      push();
      rotate(angle);
      beginShape();
      vertex(0, 0);
      vertex(L, -halfT);
      vertex(L, halfT);
      endShape(CLOSE);
      pop();
    }
    pop();
  }
}
```
</details>

### Un enlace a tu sketch en el editor de p5.js
> [Link del Sketch](https://editor.p5js.org/danipipe344/full/PYamBL-Dd)
>

### Capturas de pantalla mostrando tu pieza en acción.
><img width="350" src="https://github.com/user-attachments/assets/c39b404e-8acf-4e2b-9944-2aa02989a907" />
>

### AutoEvaluación. 
> 4.0. Entendi y realice los diferentes modificaciones y comprendi los conceptos, sin embargo el estilo o el diseño de mi obra quedo muy basico, ademas que mucho de el desarrollo del codigo mayormente esta realizado con IA. Me gustan los Caligaris
>





