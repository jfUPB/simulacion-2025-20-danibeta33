# Unidad 1

# 🔎 Fase: Set + Seek

## Actividad 01
*La belleza de lo aleatorio.*

## Actividad 02

*El papel de la aleatoriedad en el código de Sofi está en permitir la creación de raíces y un "árbol" a partir de parámetros que combinan elementos aleatorios y no aleatorios. Es interesante el hecho de que el usuario pueda experimentar esa variabilidad de forma directa le aporta un valor extra, ya que convierte la obra en una experiencia generativa distinta para cada persona que la observa.*

*Dentro del campo de los videojuegos, uso la aleatoriedad en la generación procedural de niveles, como en los roguelites. Por ejemplo, en The Binding of Isaac, aunque uno elige un personaje con ciertas características, los enemigos, cofres y habitaciones se generan de forma aleatoria, haciendo que cada partida sea una experiencia única.*


## Actividad 03
*Realicé variantes en el movimiento del punto para que fuera más aleatorio y, además, se extendiera realizando saltos.*

<details>
  <summary>Código</summary>

```js
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
</details> 

## Actividad 04
*Uniforme = todos los números tienen la misma probabilidad.*

*No uniforme = unos números poseen probabilidades mayores de ser tomados.*

<details>
  <summary>Código</summary>

```js
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
    const choice = floor(randomGaussian(0.1, 0));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
</details> 

## Actividad 05

[Link del Sketch](https://editor.p5js.org/danipipe344/full/pXRA03f5l)
<details>
  <summary>Código</summary>

```js
let colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"];
let heights = [];

function setup() {
  createCanvas(500,500);
  heights = Array(colors.length).fill(10); 
}

function draw() {
  background(255);

  
  let index = floor(random(colors.length));
  heights[index] += 1; // Crecimiento suave

  
  let y = 0;
  for (let i = 0; i < colors.length; i++) {
    fill(colors[i]);
    rect(0, y, width, heights[i]);
    y += heights[i];
  }

  
  if (y > height) {
    noLoop(); 
    console.log("El canvas se llenó con las franjas de colores.");
  }
}

```
</details> 

<img width="400" alt="image" src="https://github.com/user-attachments/assets/9cb870d2-162e-48ae-87fa-32cdc6a2f2fa" />

## Actividad 06
[Link del Sketch](https://editor.p5js.org/danipipe344/full/7QmMUiuFA)

<details>
  <summary>Código</summary>

```js
let colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"];
let heights = [];

function setup() {
  createCanvas(500, 1000);
  heights = Array(colors.length).fill(1); // Altura inicial de todas las franjas
}

function draw() {
  background(255);
  let index = floor(random(colors.length));
  heights[index] += levyStep();

  
  let y = 0;
  for (let i = 0; i < colors.length; i++) {
    fill(colors[i]);
    rect(0, y, width, heights[i]);
    y += heights[i];
  }

 
  if (y > height) {
    noLoop();
    console.log("El canvas se llenó con las franjas de colores.");
  }
}


function levyStep() {
  let r1, r2;
  let step;

  while (true) {
    r1 = random(0,2.5); 
    let probability = r1; 
    r2 = random(1); 

    if (r2 < probability) {
      step = r1;
      break; o
    }
  }
  return step;
}
```
</details> 

<img width="400" alt="image" src="https://github.com/user-attachments/assets/f26d203c-07b1-4fd8-a5c4-31fe8133cead" />
