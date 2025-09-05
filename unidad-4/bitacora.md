# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Utilice el concepto del pendulo, ademas de referenciarme de Memo Akten en su obra Simple Harmonic Motion.
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Use el concepto de fuerzas de resistenci, asi pude hacer crecer ciruclos con una aceleracion negativa, asi impedir que creciera infinitamente. 
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: Use el concepto de motion 101 para las fuerzas, asi la logica del movmiento funcionara.
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: Use el random para aqui todas las elecciones de la obra, como los colores; las masas; el tamaño; la ubicacion de los circulos. Ademas de las notas musicales que se van a tocar.
>

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí: Simplemente hice como si fuera un metronomo normal, donde se puede moficar la velocidad. Ademas modificar el velumen y por ultimo la gama de colores que se van a pintar.
>

## Enlace a la obra en el editor de p5.js

[Metronomo Colorido](https://editor.p5js.org/danipipe344/full/Mp_17i_PK)

## Código de la obra 

<details>
  <summary>Metronomo Codigo</summary>
  
```js
let tempos = [50, 100, 150, 200];
let tempoIndex = 0;
let bpm = tempos[tempoIndex];

let angle;
let angleVelocity;
let maxAngle;
let pivot;
let armLength;
let volumen = 0.3;

let monoSynth;
let audioStarted = false; 

// Círculos
let circles = [];

// Paletas armónicas
let palettes = [
  {r:[180,240], g:[80,120], b:[80,120]},   // rojos
  {r:[93,120], g:[193,240], b:[185,200]},  // verdes
  {r:[80,140], g:[100,150], b:[200,255]},  // azules
  {r:[160,200], g:[120,160], b:[200,240]}  // morados
];
let paletteIndex = 0;

function setup() {
  let cnv = createCanvas(800, 450);
  cnv.mousePressed(startAudio);

  angle = -Math.PI / 4;
  angleVelocity = 0;
  maxAngle = Math.PI / 4;
  armLength = 110;
  pivot = createVector(width / 2, height - 140);

  setTempo(bpm);

  monoSynth = new p5.MonoSynth();
  strokeCap(ROUND);
}

function draw() {
  // Fondo en tono grisáceo relacionado
  let pal = palettes[paletteIndex];
  background(
    (pal.r[0] + pal.r[1]) / 2,
    (pal.g[0] + pal.g[1]) / 2,
    (pal.b[0] + pal.b[1]) / 2,
    100
  );

  // Dibujar círculos detrás
  for (let c of circles) {
    let drag = calculateDrag(c);
    c.applyForce(drag);
    c.update();
    c.show();
  }

  // Actualizar ángulo del metrónomo
  angle += angleVelocity;

  // Extremos
  if (angle > maxAngle) {
    angle = maxAngle;
    angleVelocity *= -1;
    triggerEvent(); // nota + círculo
  } else if (angle < -maxAngle) {
    angle = -maxAngle;
    angleVelocity *= -1;
    triggerEvent(); // nota + círculo
  }

  // Dibujar metrónomo
  drawMetronomeBody();

  // Dibujar péndulo
  let bobX = pivot.x + armLength * Math.sin(angle);
  let bobY = pivot.y + armLength * Math.cos(angle);

  stroke(30);
  strokeWeight(4);
  line(pivot.x, pivot.y, bobX, bobY);

  noStroke();
  fill(20);
  circle(pivot.x, pivot.y, 10);

  stroke(0);
  strokeWeight(1.5);
  fill(180, 90, 120);
  circle(bobX, bobY, 34);

  // BPM
  noStroke();
  fill(20);
  textAlign(CENTER, BOTTOM);
  textSize(18);
  text(`Tempo: ${bpm} BPM`, width / 2, pivot.y - armLength - 18);
}

function triggerEvent() {
  playNote();
  spawnCircle();
}

function mousePressed() {
  if (audioStarted) {
    tempoIndex = (tempoIndex + 1) % tempos.length;
    bpm = tempos[tempoIndex];
    setTempo(bpm);
  }
}

function keyPressed() {
  if (key === ' ') {
    paletteIndex = (paletteIndex + 1) % palettes.length;
  }
  if (key === '+'){
    volumen += 0.2;
  }
  if (key === '-'){
    volumen -= 0.2;
  }
}

function startAudio() {
  if (!audioStarted) {
    userStartAudio();
    audioStarted = true;
  }
}

function playNote() {
  if (!audioStarted) return;
  let notes = ['C4', 'D4', 'E4', 'F4', 'G4', 'A4', 'B4'];
  let note = random(notes);
  monoSynth.play(note, volumen, 0, 0.25);
}

function spawnCircle() {
  let pal = palettes[paletteIndex];
  let col = color(
    random(pal.r[0], pal.r[1]),
    random(pal.g[0], pal.g[1]),
    random(pal.b[0], pal.b[1]),
    150
  );
  circles.push(new GrowingCircle(random(width), random(height), col));
}

class GrowingCircle {
  constructor(x, y, col) {
    this.pos = createVector(x, y);
    this.radius = 5;
    this.growth = createVector(0, random(2, 4));
    this.acc = createVector(0, 0);
    this.mass = random(0.5, 2);
    this.col = col;
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
    noStroke();
    fill(this.col);
    ellipse(this.pos.x, this.pos.y, this.radius * 2);
  }
}

function calculateDrag(circle) {
  let c = 0.05;
  let speed = circle.growth.mag();
  let dragMag = c * speed * speed;
  let drag = circle.growth.copy();
  drag.mult(-1);
  drag.setMag(dragMag);
  return drag;
}

function setTempo(bpmVal) {
  let fps = frameRate();
  if (!fps || fps <= 0 || !isFinite(fps)) fps = 60;

  let periodSec = 120 / bpmVal;
  let framesPerCycle = periodSec * fps;
  angleVelocity = (4 * maxAngle) / framesPerCycle;

  if (angle < 0) angleVelocity = Math.abs(angleVelocity);
  else angleVelocity = -Math.abs(angleVelocity);
}

function drawMetronomeBody() {
  push();
  translate(width / 2, height - 50);
  noStroke();
  fill(200);
  let w = 180;
  let h = 210;
  beginShape();
  vertex(-w / 2, 0);
  vertex(w / 2, 0);
  vertex(w / 2 - 20, -h);
  vertex(-w / 2 + 20, -h);
  endShape(CLOSE);

  stroke(50);
  strokeWeight(2);
  line(0, -h + 20, 0, 10);
  pop();
}
```
</details>

## Captura de pantalla representativa
<img width="300" src="https://github.com/user-attachments/assets/d43d1a57-d787-4b6c-af0c-b4ae1e360156" />







