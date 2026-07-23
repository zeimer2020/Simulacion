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

Mi idea es representar la torre de tesla, la que generaba energia
<img width="277" height="372" alt="image" src="https://github.com/user-attachments/assets/66ea1d47-e5e7-4dac-be1f-2db64bd0a6cd" />
(WardenClyffe tower)

en un principio generar energia comun y corriente pero cuando el mouse entre en el canvas se va a sobrecargar repetinamente, dandole un poco de interactividad al usuario 

normal 
<img width="848" height="669" alt="image" src="https://github.com/user-attachments/assets/20087fff-552d-430d-a7f4-5b2b8598609a" />

sobrecargada
<img width="886" height="651" alt="image" src="https://github.com/user-attachments/assets/9dfe9b83-9fa5-4c4a-b4cd-fe3e3de3559f" />

``` Js
let rays = [];
let particles = [];

let overload = 0;      
let mouseInside = false;

let towerTop;

function setup() {

  let cnv = createCanvas(windowWidth, windowHeight);

  cnv.mouseOver(() => {
    mouseInside = true;
  });

  cnv.mouseOut(() => {
    mouseInside = false;
  });

  towerTop = createVector(width / 2, height * 0.22);

  strokeCap(ROUND);
  angleMode(RADIANS);
}

function draw() {

  if (mouseInside) {
    overload = lerp(overload, 1, 0.08);
  } else {
    overload = lerp(overload, 0, 0.05);
  }

  drawBackground();

  drawTower();
  updateParticles();
  updateRays();
}

function drawBackground() {


  let glow = overload * 45;

  background(
    8 + glow * 0.25,
    12 + glow * 0.20,
    24 + glow * 0.35
  );

  noStroke();


  for (let i = 7; i > 0; i--) {

    fill(
      80,
      140,
      255,
      4 + overload * 12
    );

    circle(
      towerTop.x,
      towerTop.y,
      i * 120 + overload * 90
    );
  }

 

  fill(20);

  rect(
    0,
    height * 0.88,
    width,
    height * 0.12
  );
}

function updateRays() {


  let probability = lerp(0.1, 1, overload);

  if (random() < probability) {

    rays.push(
      new Lightning(
        towerTop.x,
        towerTop.y
      )
    );
  }

  for (let i = rays.length - 1; i >= 0; i--) {

    rays[i].update();
    rays[i].show();

    if (rays[i].dead) {
      rays.splice(i, 1);
    }
  }
}

function updateParticles() {

  for (let i = particles.length - 1; i >= 0; i--) {

    particles[i].update();
    particles[i].show();

    if (particles[i].dead) {
      particles.splice(i, 1);
    }
  }
}

function mouseEntered() {
  mouseInside = true;
}

function mouseExited() {
  mouseInside = false;
}

function windowResized() {

  resizeCanvas(windowWidth, windowHeight);

  towerTop.set(
    width / 2,
    height * 0.22
  );
}
function drawTower() {

  let x = width / 2;

  let top = towerTop.y + 65;
  let base = height * 0.90;

  let frontW = width * 0.12;
  let backW = frontW * 0.78;
  let depth = width * 0.035;

  let shake =
    map(noise(frameCount * 0.03), 0, 1, -2, 2) * overload;

  push();

  translate(shake, 0);


  stroke(70);
  strokeWeight(2);

  line(x - backW + depth, base, x + depth, top);
  line(x + backW + depth, base, x + depth, top);


  stroke(180);
  strokeWeight(2.5);

  line(x - frontW, base, x, top);
  line(x + frontW, base, x, top);


  stroke(90);

  line(x - frontW, base, x - backW + depth, base);
  line(x + frontW, base, x + backW + depth, base);
  line(x, top, x + depth, top);


  let levels = 28;

  for (let i = 0; i <= levels; i++) {

    let t = i / levels;

    let y = lerp(top, base, t);

    let fw = lerp(0, frontW, t);
    let bw = lerp(0, backW, t);

    stroke(170);

    line(
      x - fw,
      y,
      x + fw,
      y
    );

    stroke(80);

    line(
      x - bw + depth,
      y,
      x + bw + depth,
      y
    );

    stroke(110);

    line(
      x - fw,
      y,
      x - bw + depth,
      y
    );

    line(
      x + fw,
      y,
      x + bw + depth,
      y
    );


    if (i < levels) {

      let nt = (i + 1) / levels;

      let ny = lerp(top, base, nt);

      let nfw = lerp(0, frontW, nt);

      stroke(145);

      line(
        x - fw,
        y,
        x + nfw,
        ny
      );

      line(
        x + fw,
        y,
        x - nfw,
        ny
      );

    }

  }


  strokeWeight(3);

  stroke(190);

  line(
    x,
    top,
    x,
    top - 55
  );

  noStroke();

  fill(40);

  ellipse(
    x,
    top - 42,
    width * 0.23,
    20
  );

  stroke(120);

  strokeWeight(1);

  for (let i = -10; i <= 10; i++) {

    line(
      x + i * 5,
      top - 42,
      x + i * 2,
      top - 62
    );

  }

  noFill();

  strokeWeight(1.5);

  stroke(175);

  let domeY = top - 90;

  for (let i = 0; i < 11; i++) {

    let r = map(i, 0, 10, 18, width * 0.12);

    ellipse(
      x,
      domeY,
      r * 2,
      r * 0.75
    );

  }

  for (let a = -70; a <= 70; a += 20) {

    push();

    translate(x, domeY);

    rotate(radians(a));

    ellipse(
      0,
      0,
      width * 0.03,
      width * 0.22
    );

    pop();

  }

  let glow = 30 + overload * 50;

  noStroke();

  for (let i = 6; i > 0; i--) {

    fill(
      120,
      180,
      255,
      glow / i
    );

    circle(
      x,
      domeY,
      10 + i * 10
    );

  }

  fill(255);

  circle(
    x,
    domeY,
    12
  );

  pop();

}

// Sistema de rayos


class Lightning {

  constructor(x, y) {

    this.points = [];
    this.points.push(createVector(x, y));

    this.angle = -PI / 2 + randomGaussian() * 0.08;

    this.life = int(random(18, 35) + overload * 30);

    this.alpha = 255;

    this.dead = false;
  }

  update() {

    if (this.life <= 0) {

      this.alpha -= 18;

      if (this.alpha <= 0) {
        this.dead = true;
      }

      return;
    }

    let last = this.points[this.points.length - 1];

    this.angle += randomGaussian() * (0.08 + overload * 0.18);

    let step = random(6, 12);

    if (random() < (0.03 + overload * 0.18)) {

      step *= random(3, 6);

    }

    let next = createVector(

      last.x + cos(this.angle) * step,

      last.y + sin(this.angle) * step

    );

    this.points.push(next);

    if (random() < overload * 0.12) {

      particles.push(
        new Spark(
          next.x,
          next.y
        )
      );

    }

    this.life--;
  }

  show() {

    strokeWeight(1.8 + overload);

    stroke(
      150 + random(60),
      210 + random(45),
      255,
      this.alpha
    );

    noFill();

    beginShape();

    for (let p of this.points) {

      vertex(p.x, p.y);

    }

    endShape();



    strokeWeight(5);

    stroke(
      180,
      220,
      255,
      this.alpha * 0.12
    );

    beginShape();

    for (let p of this.points) {

      vertex(p.x, p.y);

    }

    endShape();

  }

}

class Spark {

  constructor(x, y) {

    this.pos = createVector(x, y);

    this.vel = p5.Vector.random2D();

    this.vel.mult(random(0.5, 3));

    this.life = random(15, 30);

    this.dead = false;

  }

  update() {

    this.pos.add(this.vel);

    this.vel.mult(0.97);

    this.life--;

    if (this.life <= 0) {

      this.dead = true;

    }

  }

  show() {

    noStroke();

    fill(
      180,
      220,
      255,
      this.life * 8
    );

    circle(
      this.pos.x,
      this.pos.y,
      3
    );

  }

}
function emitCorona() {

  let amount = int(lerp(1, 8, overload));

  for (let i = 0; i < amount; i++) {

    let a = random(TWO_PI);

    let r = random(8, 18);

    particles.push(
      new Spark(
        towerTop.x + cos(a) * r,
        towerTop.y - 39 + sin(a) * r
      )
    );

  }

}

setInterval(() => {

  emitCorona();

}, 80);


function drawFlash() {

  if (overload < 0.55) return;

  noStroke();

  fill(
    170,
    210,
    255,
    overload * 18
  );

  rect(
    0,
    0,
    width,
    height
  );

}

let oldDraw = draw;

draw = function () {

  oldDraw();

  drawFlash();

}
```










