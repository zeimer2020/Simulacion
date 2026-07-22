# Clase


Actividades 1, 2, 3 y 4
distribucion uniforme: todos los numeros tiene chance de salir 
distribucion no uniforme: hay una tendecnai hacia unos numeros en especifico

codigo original random walk

``` Python
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
    stroke(255, 0, 0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
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

codigo modificado

``` python

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
    stroke(255, 0, 0);
    point(this.x, this.y);
     triangle(30, 75, 58, 20, 86, 75);
  }

  step() {
    const choice = floor(random(4));
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

(la verdad no me funciono)

pd: usar random seed la proxima vez

codigo modificado distribucion hacia la derecha

``` javascript
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
    stroke(255, 0, 0);
    point(this.x, this.y);
     
  }

  step() {
    const choice = floor(random(4));
    if (choice < 1.8) {
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

``` Js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  // A normal distribution with mean 320 and standard deviation 60
  let x = randomGaussian(320, 60);
  noStroke();
  fill(10, 20);
  circle(x, 200, 16);
}
```
la verdad solo cambie un poquito la posicion pq no me estaba dando para cambiar la figura

 Actividad 5 levy flight

``` Js
let walker;

function setup() {
  createCanvas(1000, 1000);
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
    stroke(255, 0, 0);
    strokeWeight(2);
    point(this.x, this.y);
  }

  step() {
    let xstep;
    let ystep;

    let r = random(1);

    if (r < 0.1) {
      xstep = random(-100, 100);
      ystep = random(-100, 100);
    } else {
      xstep = random(-1, 1);
      ystep = random(-1, 1);
    }

    this.x += xstep;
    this.y += ystep;
  }
}
```
Actividad 6

ruido de perlin

pues es un electrocardiograma entonces segui como la misma estructura el ejemplo de la derecha y cambie el color y el fondo

``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let values = [];

function setup() {
  createCanvas(400, 260);
  for (let i = 0; i < width; i++) {
    values[i] = random(30, height - 30);
  }
}

function draw() {
  background(7, 20, 38);
  noFill();
  stroke(84, 214, 255);
  strokeWeight(3);
  beginShape();
  for (let i = 0; i < width; i++) {
    let speed = floor(frameCount * 3);
    let y = values[(i + speed) % values.length];
    vertex(i, y);
  }
  endShape();
}
```
<img width="415" height="252" alt="image" src="https://github.com/user-attachments/assets/dc1dea50-4f78-4b70-ad43-040ffe76d8a4" />
no se ve pero se esta moviendo mas rapido 



Actividad 7

Mi idea es que en la pantalla hayan multiples puntos que salgan de manera automatica y que la interaccion del usuario sea colocar el mouse en el canvas, lo que generara un aumento de velocidad de la aparicion de los puntos  

hice varias versiones, una pidiendo correcciones y ayudaspromteando con la IA y otra directamente prompteando con la idea que tenia originalmente

primera version
``` js
let walker;
let mouseInside = false;

function setup() {
  let canvas = createCanvas(windowHeight * 9 / 16, windowHeight);

  canvas.mouseOver(() => mouseInside = true);
  canvas.mouseOut(() => mouseInside = false);

  walker = new Walker();
  background(7, 20, 38);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.bigStep = false;
  }

  show() {
    if (this.bigStep) {
      stroke(255, 92, 138);
      strokeWeight(5);
    } else {
      stroke(84, 214, 255);
      strokeWeight(2);
    }

    point(this.x, this.y);
  }

  step() {
    
    let ratio = mouseInside ? 1.10 : 0.01;

    this.bigStep = random(1) < ratio;

    let xstep;
    let ystep;

    if (this.bigStep) {
      xstep = random(-100, 100);
      ystep = random(-100, 100);
    } else {
      xstep = random(-1, 1);
      ystep = random(-1, 1);
    }

    this.x += xstep;
    this.y += ystep;

    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }
}

```


<img width="463" height="769" alt="image" src="https://github.com/user-attachments/assets/65fd203b-38ba-4822-af89-689094a24786" />

Mi idea aca era mas como que fueran particulas que al ser afectadas cambiarian de color y medio seguirian al mouse

segunda version prompteada

``` Js
let walker;
let mouseInside = false;
let waves = [];

function setup() {
  let canvas = createCanvas(windowHeight * 9 / 16, windowHeight);

  canvas.mouseOver(() => mouseInside = true);
  canvas.mouseOut(() => mouseInside = false);

  background(0);

  walker = new Walker();
}

function draw() {

  // Estela
  noStroke();
  fill(0, 18);
  rect(0, 0, width, height);

  walker.step();
  walker.show();

  // Dibujar ondas
  for (let i = waves.length - 1; i >= 0; i--) {
    waves[i].update();
    waves[i].show();

    if (waves[i].dead()) {
      waves.splice(i, 1);
    }
  }
}

class Walker {

  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.bigStep = false;
  }

  step() {

    let ratio = mouseInside ? 0.15 : 0.008;
    this.bigStep = random() < ratio;

    let xstep, ystep;

    if (mouseInside) {

      let dx = mouseX - this.x;
      let dy = mouseY - this.y;

      let d = dist(this.x, this.y, mouseX, mouseY);

      if (d > 0) {
        dx /= d;
        dy /= d;
      }

      if (this.bigStep) {
        xstep = random(-60, 60);
        ystep = random(-60, 60);
      } else {
        xstep = random(-3, 3);
        ystep = random(-3, 3);
      }

      xstep += dx * 2.2;
      ystep += dy * 2.2;

      if (d < 80) {
        xstep += random(-2.5, 2.5);
        ystep += random(-2.5, 2.5);
      }

    } else {

      // Movimiento automático
      xstep = random(-3, 3);
      ystep = random(-3, 3);

      if (frameCount % 60 === 0) {
        xstep += random(-60, 60);
        ystep += random(-60, 60);
      }

      if (random() < 0.03) {
        xstep += random(-35, 35);
        ystep += random(-35, 35);
      }

    }

    this.x += xstep;
    this.y += ystep;

    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);

  }

  show() {

    if (this.bigStep) {

      noStroke();

      if (mouseInside) {

        // Amarillo cálido
        fill(255, 210, 80, 35);
        circle(this.x, this.y, 18);

        fill(255, 220, 100, 90);
        circle(this.x, this.y, 10);

        fill(255, 245, 210);
        circle(this.x, this.y, 4);

      } else {

        // Azul eléctrico
        fill(120, 220, 255, 30);
        circle(this.x, this.y, 18);

        fill(120, 220, 255, 90);
        circle(this.x, this.y, 10);

        fill(255);
        circle(this.x, this.y, 4);

      }

    } else {

      if (mouseInside) {
        stroke(255, 215, 90, 180);
      } else {
        stroke(120, 220, 255, 180);
      }

      strokeWeight(random(1, 2));
      point(this.x, this.y);

    }

    if (mouseInside || random() < 0.15) {
      waves.push(new Wave(this.x, this.y, this.bigStep));
    }

  }

}

class Wave {

  constructor(x, y, fast) {

    this.x = x;
    this.y = y;
    this.fast = fast;

    this.radius = 2;

    if (fast) {
      this.maxRadius = 18;
      this.speed = 3.8;
      this.alpha = 220;
    } else {
      this.maxRadius = 30;
      this.speed = 0.7;
      this.alpha = 55;
    }

  }

  update() {

    this.radius += this.speed;

    if (this.fast) {
      this.alpha -= 10;
    } else {
      this.alpha -= 2;
    }

  }

  show() {

    noFill();

    if (this.fast) {

      strokeWeight(2);

      if (mouseInside) {

        stroke(255, 215, 90, this.alpha);
        circle(this.x, this.y, this.radius * 2);

        stroke(255, 245, 200, this.alpha * 0.5);
        circle(this.x, this.y, this.radius * 1.6);

      } else {

        stroke(140, 230, 255, this.alpha);
        circle(this.x, this.y, this.radius * 2);

        stroke(255, 255, 255, this.alpha * 0.5);
        circle(this.x, this.y, this.radius * 1.6);

      }

    } else {

      strokeWeight(1);

      if (mouseInside) {
        stroke(255, 210, 80, this.alpha);
      } else {
        stroke(80, 170, 255, this.alpha);
      }

      circle(this.x, this.y, this.radius * 2);

    }

  }

  dead() {
    return this.radius > this.maxRadius || this.alpha <= 0;
  }

}

```

<img width="418" height="681" alt="image" src="https://github.com/user-attachments/assets/7d04da5f-8af5-4f57-a4cf-b5ad095cb83a" />

<img width="358" height="665" alt="image" src="https://github.com/user-attachments/assets/a53e143f-9ff9-4b77-a718-7104b1628e7a" />






















