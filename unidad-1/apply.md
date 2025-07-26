# Unidad 1
## 🛠 Fase: Apply

*En el sketch se buscó implementar la aleatoriedad de los walkers, pero manteniéndoles una limitación: los ejes en los que podían trasladarse. Sin embargo, el usuario, mediante el teclado, es capaz de modificar ese eje, además de cambiar los colores, manteniéndolos dentro de una paleta de colores convergente con los demás. El resultado es un programa en el que walkers se desplazan en distintos ejes, pintando con colores diferentes pero pertenecientes a una misma escala cromática, mientras el usuario los manipula para generar una obra completamente única*

<details>
  <summary>Código</summary>

```js
let t = 0;
let t2 = 100;
let t3 = 200;
let t4 = 300;

let angleX = 0;
let angleY = 0;

let colorHorizontal, colorVertical, colorDiag1, colorDiag2;
let basePalette;

function setup() {
  createCanvas(600, 400);
  background(255);
  noStroke();

  setNewPalette();
}

function draw() {
  fill(220, 220, 220, 2); 
  rect(0, 0, width, height);

  translate(width / 2, height / 2);

  let n = noise(t);
  let x = map(n, 0, 1, -width / 2, width / 2);
  push();
  rotate(angleX);
  fill(colorHorizontal);
  circle(x, 0, 16);
  pop();

  let n2 = noise(t2);
  let y = map(n2, 0, 1, -height / 2, height / 2);
  push();
  rotate(angleY);
  fill(colorVertical);
  circle(0, y, 16);
  pop();

  let n3 = noise(t3);
  let d1 = map(n3, 0, 1, -width / 2, width / 2);
  push();
  rotate(PI / 4 + angleY);
  fill(colorDiag1);
  circle(d1, 0, 16);
  pop();

  let n4 = noise(t4);
  let d2 = map(n4, 0, 1, -width / 2, width / 2);
  push();
  rotate(-PI / 4 + angleX);
  fill(colorDiag2);
  circle(d2, 0, 16);
  pop();

  t += 0.01;
  t2 += 0.01;
  t3 += 0.01;
  t4 += 0.01;
}

function randomColorFromPalette(meanColor) {
  let r = constrain(meanColor[0] + randomGaussian() * 30, 0, 255);
  let g = constrain(meanColor[1] + randomGaussian() * 30, 0, 255);
  let b = constrain(meanColor[2] + randomGaussian() * 30, 0, 255);
  return color(r, g, b);
}

function setNewPalette() {
 
  basePalette = [random(255), random(255), random(255)];

  // Colores similares entre sí
  colorHorizontal = randomColorFromPalette(basePalette);
  colorVertical   = randomColorFromPalette(basePalette);
  colorDiag1      = randomColorFromPalette(basePalette);
  colorDiag2      = randomColorFromPalette(basePalette);
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    angleX -= PI / 60;
  } else if (keyCode === RIGHT_ARROW) {
    angleX += PI / 60;
  } else if (keyCode === UP_ARROW) {
    angleY -= PI / 60;
  } else if (keyCode === DOWN_ARROW) {
    angleY += PI / 60;
  } else if (key === ' ') {
    setNewPalette(); 
  }
}
```
</details> 

[Link del Sketch](https://editor.p5js.org/danipipe344/full/jhuvzVIb9)

<img width="400" alt="image" src="https://github.com/user-attachments/assets/2946b5e9-ff03-4107-9dd4-37fca95aef77" />
