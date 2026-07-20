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
