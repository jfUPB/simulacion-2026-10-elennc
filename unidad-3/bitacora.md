# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 01

Todo el tema de la IA la verdad me angustia. Toda mi vida he estado rodeada de arte, de parte de mis papás artistas, mis tíos, mi hermano, mis amigos de pequeña, siempre he sabido qué quiero hacer con mi vida, en un sentido general. Me enoja que exista gente que piense que puede replicar lo que yo he trabajado toda mi vida por llegar a ser, solo porque ahora hay un programa que piensa por ellos. Yo no considero que el arte generado por IA sea arte real, pues para mí le hace falta el alma de un artista. Y sí, es indiscutible que las personas que lograron crear estas máquinas tan locas son increiblemente talentosas, mas no creo justo que se metan en la misma bolsa de los artistas.

Esto igual no me parece que aplica a las personas que realmente crean arte computacional, como hacemos nosotros en este curso, entendiendo cada línea y cambiando requerimientos del algoritmo hasta obtener algo que deseamos, porque al final es un humano utilizando el computador como herramienta, mientras que alguien que escribe “dame una flor” y recibe una flor simplemente está demandando, perdiéndose la parte del arte que le da sentido a la vida, que es el proceso y la expresión en su más pura forma. Yo seguiré creando de la forma tradicional porque eso es lo que me hace humana y de ahí es que saco las ganas de vivir todos los días, pero sí me pone triste saber que otras personas ya no aprecian ese esfuerzo por querer una gratificación instantánea.

### Actividad 02

```jsx
update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
}
```

*¿Por qué es necesario multiplicar la aceleración por cero en cada frame?*

*¿Por qué se multiplica por cero **justo al final** de update()?*

Se calcula la aceleración al utilizar el método applyForce( ) del mover, y usamos esa aceleración para la velocidad y esa velocidad para la posición, y después de eso necesitamos que vuelva a resetearse para el siguiente frame, en vez de acumularse. Si no lo multiplicamos por 0 al final no se anula y cada vez se calcularía una suma infinita de fuerza más fuerza más fuerza más fuerza.

Si la masa no es 1, no puedo simplemente modificar la fuerza dividiéndola por la masa ya que esto cambia el valor global de la fuerza y no la podríamos volver a usar, por lo que debemos pasarla por referencia, o sea como crear una copia de ese valor en una variable nueva, y este sí podemos dividirlo entre la masa y así solo usarlo para encontrar la aceleración en el mpetodo de applyForce( ).

### Actividad 03

FRICCIÓN:

Para este tipo de fuerza decidí hacer algo que demostrara que la fricción actúa en contra del movimiento haciendo que el objeto tienda a quedarse quieto después de un tiempo, y me pareció divertido visualizarlo como un juego de bolos en el que la bola tiene una fricción al moverse hacia los pinos y al golpearlos estos también se ven afectados por la fricción, deslizándose solo un poco antes de detenerse. 

```jsx
let ball;
let pins = [];

function setup() {
  createCanvas(800, 400);

  // Pelota
  ball = new Mover(100, height/2, 5, 20, "ball");
  ball.velocity.x = 10;

  // Formación triangular de pinos
  let startX = 480;
  let startY = height /2;
  let spacing = 35;

  for (let row = 0; row < 4; row++) {
    for (let i = 0; i <= row; i++) {
      pins.push(
        new Mover(
          startX + row * spacing,
          startY - i * spacing + row * spacing / 2,
          1,
          12,
          "pin"
        )
      );
    }
  }
}

function draw() {
  background(15, 20, 30, 40); 

  // Pelota
  ball.applyFriction();
  ball.update();
  ball.show();

  // Colisión pelota con pinos
  for (let pin of pins) {
    collide(ball, pin);
  }

  // Pinos
  for (let pin of pins) {

    // Colisiones entre pinos
    for (let other of pins) {
      if (pin !== other) {
        collide(pin, other);
      }
    }

    pin.applyFriction();
    pin.update();
    pin.show();
  }
}

function collide(a, b) {
  let distance = p5.Vector.dist(a.position, b.position);

  if (distance < a.radius + b.radius) {

    let force = p5.Vector.sub(a.position, b.position);
    force.normalize();
    force.mult(1.5);

    a.applyForce(force);
    b.applyForce(force.copy().mult(-1));
  }
}

```
![u3a3v1](https://github.com/user-attachments/assets/b74a054b-4667-43fe-a050-58c590e13c69)



RESISTENCIA AL AIRE Y LÍQUIDOS:

Para este ejemplo hice una escena de agua y aire, en el que partículas caen con una ligera resistencia al aire dependiendo de su masa y al llegar al agua esta resistencia aumenta aún más, haciendo que se muevan más lento y parezca que se hunden.

```jsx
class Mover {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = random(1, 3);
    this.radius = this.mass * 6;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  calculateDrag(c) {
    let speed = this.velocity.mag();

    let drag = this.velocity.copy();
    drag.mult(-1);
    drag.normalize();
    drag.mult(c * speed * speed);

    return drag;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);

    if (this.position.y > height) {
      this.position.y = random(-100, 0);
      this.velocity.mult(0);
    }
  }

  show() {
    let speed = this.velocity.mag();

    fill(map(speed, 0, 10, 100, 255), 180, 255, 180);
    noStroke();
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }
}
```
![u3a3v2](https://github.com/user-attachments/assets/b85565bb-750f-4f50-a066-294aae02b641)



ATRACCIÓN GRAVITACIONAL:

Para este quise tomármelo literal y hacer un planeta que atrae un cohete, pero para que fuera más interesante hice que se activara la atracción al darle click.

```jsx
let rocketPos;
let rocketVel;
let rocketAcc;

let planetPos;
let planetMass = 2000;

let G = 0.5;
let attracting = false;

function setup() {
  createCanvas(600, 600);

  planetPos = createVector(width/2, height/2);

  rocketPos = createVector(random(width), random(height));
  rocketVel = p5.Vector.random2D().mult(2);
  rocketAcc = createVector(0, 0);
}

function draw() {
  background(10);

  // Dibujar planeta
  noStroke()
  
  fill(100, 150, 255);
  circle(planetPos.x, planetPos.y, 80);
  
  fill('rgb(180,180,242)')
  ellipse(planetPos.x,planetPos.y, 130, 50)
  
  fill('rgb(124,124,233)')
  ellipse(planetPos.x,planetPos.y-3, 110, 40)
  
  fill('rgb(180,180,242)')
  ellipse(planetPos.x,planetPos.y-3, 100, 35)
  
  fill(100, 150, 255);
  ellipse(planetPos.x, planetPos.y-13, 73, 40);

  // Aplicar gravedad solo si se hace click
  if (attracting) {
    let force = p5.Vector.sub(planetPos, rocketPos);
    let distance = force.mag();

    // evitar fuerza infinita
    distance = constrain(distance, 20, 300);

    force.normalize();

    let strength = (G * planetMass) / (distance * distance);
    force.mult(strength);

    rocketAcc.add(force);
  }

  // Actualizar física
  rocketVel.add(rocketAcc);
  rocketPos.add(rocketVel);

  rocketAcc.mult(0);

  drawRocket();

  // Reset si sale del canvas
  if (
    rocketPos.x < -50 || rocketPos.x > width + 50 ||
    rocketPos.y < -50 || rocketPos.y > height + 50
  ) {
    rocketPos = createVector(random(width), random(height));
    rocketVel = p5.Vector.random2D().mult(2);
  }
}

function drawRocket() {
  push();
  translate(rocketPos.x, rocketPos.y);

  rotate(rocketVel.heading() + PI/2);

  noStroke()
  fill('#FF9800')
  triangle(5, 50, 10, 80, 15, 50);
  triangle(5, 50, -6, 70, 15, 50);
  triangle(5, 50, 24, 70, 15, 50);
  
  fill('#FFEB3B')
  triangle(8, 50, 10, 70, 12, 50);
  triangle(8, 50, 1, 60, 12, 50);
  triangle(8, 50, 19, 60, 12, 50);
  
  fill('rgb(223,216,216)')
  rect(0, 0, 20, 50)
  triangle(0, 0, 10, -30, 20, 0);
  
  fill('#E91E63')
  triangle(0, 30, 0, 50, -12, 60);
  triangle(20, 30, 20, 50, 32, 60);
  
  stroke('rgb(46,44,44)')
  fill('#A8DFF8')
  circle(10, 4, 13)
  pop();
}

function mousePressed() {
  attracting = true;
}

function mouseReleased() {
  attracting = false;
}
```

![u3a3v3](https://github.com/user-attachments/assets/c3279167-217c-40a3-8301-fac05f2a0957)




## Bitácora de aplicación 

### Actividad 04

Nuevamente quise hacer algo relacionado con insectos y seres vivos porque para mí algo vivo es la mejor forma de visualizar la física. Esta vez hice una mariquita que tiene atracción al mouse al darle click, pero además hay una fuerza gravitacional desde el trébol en el centro de la pantalla que la atrae cuando no se está presionando nada. A su vez, cuando se mueve, tiene una resistencia al aire que hace que tienda a quedarse quieta.

```jsx
let ladyPos;
let ladyVel;
let ladyAcc;

let decoX = [];
let decoY = [];
let decoSize = [];

let cloverPos;

let G = 0.3;
let cloverMass = 1000;

let mouseForceStrength = 2.5;

let dragCoefficient = 0.02;

let captureRadius = 20;

function setup() {
  createCanvas(600, 580);

  randomSeed(10); // siempre misma distribución

  let spacing = 120;

  for (let x = spacing/2; x < width; x += spacing) {
    for (let y = spacing/2; y < height; y += spacing) {

      let offsetX = random(-25, 25);
      let offsetY = random(-25, 25);

      // evitar zona central
      let d = dist(x, y, width/2, height/2);

      if (d > 120) {
        decoX.push(x + offsetX);
        decoY.push(y + offsetY);
        decoSize.push(random(18, 30));
      }
    }
  }

  cloverPos = createVector(width/2, height/2);

  ladyPos = createVector(random(width), random(height));
  ladyVel = p5.Vector.random2D().mult(2);
  ladyAcc = createVector(0, 0);
}

function draw() {
  background(46, 85, 46);

  drawBackgroundClovers();
  drawClover();

  let distanceToClover = p5.Vector.dist(ladyPos, cloverPos);

  // 🌸 ZONA DE CAPTURA
  if (distanceToClover < captureRadius && !mouseIsPressed) {
    ladyPos = cloverPos.copy();
    ladyVel.mult(0);
    ladyAcc.mult(0);
    drawLadybug();
    return;
  }

  // 💨 RESISTENCIA DEL AIRE
  let speed = ladyVel.mag();
  let dragMagnitude = dragCoefficient * speed * speed;

  if (speed > 0) {
    let drag = ladyVel.copy();
    drag.mult(-1);
    drag.normalize();
    drag.mult(dragMagnitude);
    ladyAcc.add(drag);
  }

  // 🍀 GRAVEDAD SUAVE AL TRÉBOL
  let gravityForce = p5.Vector.sub(cloverPos, ladyPos);
  let distance = gravityForce.mag();
  distance = constrain(distance, 30, 300);

  gravityForce.normalize();
  let strength = (G * cloverMass) / (distance * distance);
  gravityForce.mult(strength);

  ladyAcc.add(gravityForce);

  // 🖱 FUERZA HACIA EL MOUSE
  if (mouseIsPressed) {
    let mouseForce = createVector(mouseX, mouseY);
    mouseForce.sub(ladyPos);
    mouseForce.normalize();
    mouseForce.mult(mouseForceStrength);
    ladyAcc.add(mouseForce);
  }

  // ACTUALIZAR FÍSICA
  ladyVel.add(ladyAcc);
  ladyPos.add(ladyVel);
  ladyAcc.mult(0);

  drawLadybug();

  // reaparece si se va
  if (ladyPos.x < -50 || ladyPos.x > width+50 ||
      ladyPos.y < -50 || ladyPos.y > height+50) {
    ladyPos = createVector(random(width), random(height));
    ladyVel = p5.Vector.random2D().mult(2);
  }
}

function drawLadybug() {
  push();
  translate(ladyPos.x, ladyPos.y);
  rotate(ladyVel.heading());

  noStroke();

  fill(220, 30, 30);
  ellipse(0, 0, 30, 25);

  fill(0);
  ellipse(12, 0, 12, 16);

  fill(0);
  ellipse(3, -7, 5);
  ellipse(3, 7, 5);
  ellipse(-8, -6, 5);
  ellipse(-8, 6, 5);
  ellipse(-1, 0, 7);

  fill(255);
  ellipse(6, 0, 3);

  fill(255);
  ellipse(13, -6, 7, 4);
  ellipse(13, 6, 7, 4);

  pop();
}

function drawClover() {
  push();
  translate(cloverPos.x, cloverPos.y);

  noStroke();
  fill(40, 140, 60);
  rect(-3, 60, 6, 40);

  fill(50, 180, 70);
  for (let i = 0; i < 4; i++) {
    ellipse(30, 0, 70, 60);
    rotate(HALF_PI);
  }

  fill(180, 224, 185);
  for (let i = 0; i < 4; i++) {
    ellipse(20, 0, 40, 30);
    rotate(HALF_PI);
  }

  fill(50, 180, 70);
  for (let i = 0; i < 4; i++) {
    ellipse(14, 0, 40, 31);
    rotate(HALF_PI);
  }

  pop();
}

function drawBackgroundClovers() {
  for (let i = 0; i < decoX.length; i++) {
    push();
    translate(decoX[i], decoY[i]);
    scale(decoSize[i] / 40);

    noStroke();
  fill(40, 140, 60);
  rect(-3/2, 60/2, 6/2, 40/2);

  fill(50, 180, 70);
  noStroke();

  for (let i = 0; i < 4; i++) {
    ellipse(30/2, 0, 70/2, 60/2);
    rotate(HALF_PI);
  }
  fill(180, 224, 185);
  noStroke();
  for (let i = 0; i < 4; i++) {
    ellipse(20/2, 0, 40/2, 30/2);
    rotate(HALF_PI);
  }
  fill(50, 180, 70);
  noStroke();
  for (let i = 0; i < 4; i++) {
    ellipse(14/2, 0, 40/2, 31/2);
    rotate(HALF_PI);
  }
    pop();
  }
}
```
![u3a4](https://github.com/user-attachments/assets/c017aac0-bd0a-4d03-a664-ec33e3ae9586)


[LINK AL PROYECTO](https://editor.p5js.org/elennc/full/cl41s3Bjo)

## Bitácora de reflexión
