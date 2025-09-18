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

[Enlace Ejemplo 4.2](https://editor.p5js.org/danipipe344/full/ToGpPGcIy)

<img width="300" src="https://github.com/user-attachments/assets/15630ac2-19b9-4e8b-a7f7-5c9f46465e9f" />

> El ejercicio ya mantenia el sistema de eliminacion de particulas, esta se maneja on un timepo de vida, donde se revisa un array dee las particulas, y las borra en el orden contrario. **Los cambios** que hice fue poner un noise, debido a la sencilles del ejercicio me parecio correcto simplemente cambiar los colores de la particula mientras viven, ademas para poder visualizarlo bien agrande el canvas y alargue la vida de las particulas.
>


### Ejemplo 4.4: a System of Systems.

<details>
  <summary>Codigo Ejemplo 4.4</summary>
  
```js
```
</details>

[Enlace Ejemplo 4.4](https://chatgpt.com/c/68cb4dfd-4c20-8323-9e93-d28596440a84)

### Ejemplo 4.5: a Particle System with Inheritance and Polymorphism.

<details>
  <summary>Codigo Ejemplo 4.5</summary>
  
```js
```
</details>

[Enlace Ejemplo 4.5](https://chatgpt.com/c/68cb4dfd-4c20-8323-9e93-d28596440a84)

### Ejemplo 4.6: a Particle System with Forces.

<details>
  <summary>Codigo Ejemplo 4.6</summary>
  
```js
```
</details>

[Enlace Ejemplo 4.6](https://chatgpt.com/c/68cb4dfd-4c20-8323-9e93-d28596440a84)

### Ejemplo 4.7: a Particle System with a Repeller.

<details>
  <summary>Codigo Ejemplo 4.7</summary>
  
```js
```
</details>

[Enlace Ejemplo 4.7](https://chatgpt.com/c/68cb4dfd-4c20-8323-9e93-d28596440a84)

