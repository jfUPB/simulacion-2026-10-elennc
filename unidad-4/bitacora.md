# Unidad 4

## Bitácora de proceso de aprendizaje
### Actividad 01

Su obra me recuerda mucho a los screensavers que veía en el computador de mi casa cuando estaba pequeña, en los 2000s, que servían como forma de visualizar el tiempo y aún así mantenerte activo mentalmente, en cierta forma. Es como el logo de DVD en un televisor, que rebota contra los bordes y siempre esperas que pegue contra la esquina, pero él sigue un patrón que hace que no sea muy común que suceda, pero igual quieres seguir mirándolo. A eso me recuerda.

### Actividad 02

- *¿Qué está pasando en esta simulación? ¿Cuál es la interacción?*
- *Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?*
- *Cuál es la relación entre el sistema de coordenadas y la función `rotate()`.*

```jsx
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let angle = 0;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  fill(127);
  stroke(0);
  
  push();  
  rectMode(CENTER);
  translate(width / 2, height / 2);
  rotate(angle);
  line(-50, 0, 50, 0);
  stroke(0);
  strokeWeight(2);
  fill(127);
  circle(50, 0, 16);
  circle(-50, 0, 16);
  pop();
  
  
  // 
   //angle += 0.1;
}

function keyPressed(){
  angle += 0.1;
  
}
```

En esta simulación tenemos una línea que conecta dos círculos, y cada que presionamos una tecla la línea rota el sistema 0.1 unidades en el sentido positivo con respecto a la horizontal como punto de inicio.

En cada frame se traslada el sistema de coordenadas a la mitad de la pantalla, ya que por default p5js tiene el origen en la esquina superior derecha y si no trasladamos cada que hacemos un dibujo, por así decirlo, pues solo veríamos un pedazo muy pequeño o incluso estaría toda la figura fuera de la pantalla en vez de verlo girar con origen en el centro. Por eso trasladamos cada vez el sistema de coordenadas a (width/2, height/2), y si no hiciéramos esto tendríamos que estar cambiando la posición de los elementos como (200,-100) o algo similar para poder tenerlo en el centro, y haciendo operaciones matemáticas con estos numeros; si directamente trasladamos el sistema completo nos ahorramos las operaciones puesto que el estándar (0, 0) ahora se encuentra donde queremos que esté la figura. El sistema de coordenadas y la función rotate( ) se relacionan ya que esta función toma el punto de origen como el eje de rotación.

En el código dado el marco de motion 101 está en la función update:

```jsx
update() {
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    dir.normalize();
    dir.mult(0.5);
    this.acceleration = dir;

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }
```

En este marco se está creando un vector que da la posición del mouse y este se le resta a la posición del mover para después normalizarlo, haciendo que quedemos con un vector que sale del mover y apunta con dirección al mouse en todo momento con una magnitud de 0.5. Después este vector se utiliza como la dirección de la aceleración del mover, y con la aceleración hallamos la velocidad y con la velocidad hallamos la posición (que es motion 101).

```jsx
  display() {
    let angle = this.velocity.heading();

    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    rectMode(CENTER);
    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);

    pop();
  }
```

- heading( ) devuelve el ángulo que hay entre el vector (en este caso el de velocidad) y la horizontal y=0
- push( ) y pop( ) son funciones que se utilizan para “agrupar” las funciones de dibujo, como tenerlas en una carpeta y que todas se vean afectadas por las líneas anteriores (como stroke, strokeWeight y fill)
- Por default, las funciones de square( ) y rect( ) utilizan la esquina superior izquierda como punto de referencia, entonces para modificar el tamaño hay que tener presente este punto como el origen y calcular desde ahí. rectMode( ) permite cambiar este punto de referencia, por ejemplo poniéndolo en el centro con rectMode(CENTER) y haciendo que sea un poquito más fácil de entender cómo modificar la figura para llegar a lo que queremos. (básicamente)
- El vector velocidad siempre apunta en la dirección del movimiento, así que si el movimiento es una curva significa que la velocidad está cambiando constantemente de dirección. La velocidad angular se puede definir como el ángulo que se recorre por segundo.



### Actividad 03

Utilizando el código de la Actividad 02 como base:

```jsx
  // The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = 0;
    this.topspeed = 4;
    this.xoff = 1000;
    this.yoff = 0;
    this.r = 16;
  }

  update() {
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    dir.normalize();
    dir.mult(0.5);
    this.acceleration = dir;

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }

  show() {
    let angle = this.velocity.heading();

    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    rectMode(CENTER);

    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);

    pop();
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```

No necesito nada de lo que tiene que ver con la posición del mouse, así que puedo reescribir la función de update.

```jsx
update() {
    this.acceleration=0
    // Controles
    if (keyIsDown(LEFT_ARROW)) {
      this.acceleration = -0.2;
    }

    if (keyIsDown(RIGHT_ARROW)) {
      this.acceleration = 0.2;
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }
```

Primero hacemos que en el inicio de cada frame la aceleración sea 0, para que esta no sea acumulativa. Después escribimos condicionales que leen qué tecla se está presionando y dependiendo de esto se asigna una magnitud y dirección a la velocidad, y esta última afecta la posición.

Para hacer que el triángulo apunte en la dirección del movimiento debemos estar referenciando el ángulo que hace el vector movimiento con la horizontal, y rotando nuestro sistema de coordenadas en ese ángulo. Como en este ejemplo solo se mueve a la derecha e izquierda, entonces el ángulo es 0 o 180 respectivmente ya que por default (ángulo 0) el triángulo apunta a la derecha.

```jsx
show() {
    let angle = this.velocity.heading();

    noStroke();
    fill('purple');
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    triangle(25, 0, -15, 15, -15, -15)
    pop();
  }
```

Dibujamos dentro de las funciones de push( ) y pop( ) para que queden agrupadas.

Este es el código final:

```jsx
class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = 0;
    this.topspeed = 4;
    
  }

  update() {
    this.acceleration=0
    // Controles
    if (keyIsDown(LEFT_ARROW)) {
      this.acceleration = -0.2;
    }

    if (keyIsDown(RIGHT_ARROW)) {
      this.acceleration = 0.2;
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }

  show() {
    let angle = this.velocity.heading();

    noStroke();
    fill('purple');
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    triangle(25, 0, -15, 15, -15, -15)
    pop();
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```

![u4a3.gif](attachment:966faa59-2943-4b6f-952f-3a630b234aeb:u4a3.gif)

### Actividad 04

Cuando se quiere agregar una fuerza acumulativa, debemos calcular esta fuerza y aplicarla sumándosela a la aceleración ya existente (si es que la hay), siguiendo la primera ley de Newton.

El attractor está en su propio file, y se le puede cambiar el color en su función de display( ).

Para poder hacer que el attractor cambie de color cuando el mouse está sobre él y que se pueda arrastrar por la pantalla, debemos verificar el estado del mouse, para saber su posición y si se está presionando o no. Para esto creamos un atributo del attractor que se llama checkMouse( )

```jsx
checkMouse() {
  let d = dist(mouseX, mouseY, this.position.x, this.position.y);

  if (d < this.mass) {
    this.rollover = true;
  } else {
    this.rollover = false;
  }
}
```

Aquí calculamos con dist( ) la posición del mouse y la comparamos con la del attractor. Si la diferencia entre las posiciones es menor a mass, que es una variable que utilizamos como el radio del attractor a la hora de dibujarlo, entonces asumimos que el mouse sí está encima del objeto, cambiando el boolean rollover a true.

Después, debemos checkear si el mouse está siendo presionado o no. Para esto creamos una función fuera de la clase del atractor llamada mousePressed( ) y una complementaria para checkear si se dejó de presionar, llamada mouseReleased( )

```jsx
function mousePressed() {
  let d = dist(mouseX, mouseY, attractor.position.x, attractor.position.y);
  
  if (d < attractor.mass) {
    attractor.dragging = true;
  }
}

function mouseReleased() {
  attractor.dragging = false;
}
```

En mousePressed( ) volvemos a calcular la distancia del attractor al mouse pero ahora si la distancia es menor al radio y asumimos que el mouse está encima del attractor, cambiamos el boolean dragging a true. En mouseReleased( ), el boolean vuelve a hacerse negativo. Ahora, sabiendo si se está presionando el mouse o no, podemos referenciar el atributo de dragging en un método nuevo de la clase de attractor en el que, si dragging es verdadero, entonces la posición del attractor será la misma que la del mouse. Como este atributo después se llama en el update, se actualiza constantemente y hace que podamos arrastrar el attractor por la pantalla.

```jsx
drag() {
  if (this.dragging) {
    this.position.x = mouseX;
    this.position.y = mouseY;
  }
}
```

En el método de display( ) del attractor podemos utilizar dragging y rollover como condicionales para cambiar el color; si dragging=true (es decir, si se está presionando el mouse sobre el attractor) el color es morado, si rollover=true (si el mouse está sobre el attractor, la distancia al mouse es menor al radio) el color es azul, y si no cumple ninguna de las condiciones el objeto por default es rosado.

```jsx
display() {
    ellipseMode(CENTER);
    stroke(255);
    if (this.dragging) {
      fill('purple');
    } else if (this.rollover) {
      fill('rgb(168,168,227)');
    } else {
      fill('rgb(241,99,123)');
    }
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
```

![u4a4.gif](attachment:3935805b-ceb1-4b8c-83cd-0244e3672b50:u4a4.gif)

### Actividad 05

```jsx
function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  // Convert polar to cartesian
  let x = r * cos(theta);
  let y = r * sin(theta);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(x, y, 48);
  theta += 0.02;
}
```

La relación entre tetha y r está basada en el teorema de pitágoras, siendo x el cateto adyacente, y el cateto opuesto y r la hipotenusa. Para hallar tetha utilizamos el teorema de pitágoras (cosθ=x/r, senθ=y/r) llegando a x=r*cosθ y y=r*senθ.

Modificando draw( ):

```jsx
function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

Al hacer este cambio hay un error, ya que a la hora de llamar la función line( ) estamos utilizando las variables x, y como la posición de la línea, pero ya no están definidas. En su lugar tenemos un vector con un ángulo tetha y magnitud nula.

Modificando nuevamente:

```jsx
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta,r);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, v.x, v.y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

Ahora sí estamos refiriéndonos al vector creado a partir de tetha para crear la línea, y sabemos que este vector tiene una magnitud de r. Como tetha +=0.02 al final de la función, la posición cambia por 0.02° en cada ciclo.

### Actividad 06

```jsx
let amplitude = 80;
let period = 120;
let phase = 0;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  let omega = TWO_PI / period;
  let frequency = 1 / period;

  translate(width/2, height/2);

  // dibujar eje
  stroke(200);
  line(-width/2, 0, width/2, 0);

  // dibujar onda seno
  stroke(0);
  noFill();
  beginShape();

  for (let x = -width/2; x < width/2; x++) {
    let angle = omega * (frameCount + x) + phase;
    let y = amplitude * sin(angle);
    vertex(x, y);
  }

  endShape();

  // punto que oscila
  let x = 0;
  let y = amplitude * sin(omega * frameCount + phase);

  fill('pink');
  circle(x, y, 20);

  // texto informativo
  resetMatrix();
  fill(0);
  noStroke();

  text("Amplitud (Q/A): " + amplitude, 10, 20);
  text("Periodo (W/S): " + period, 10, 40);
  text("Fase (E/D): " + phase.toFixed(2), 10, 100);
  text("Frecuencia: " + frequency.toFixed(3), 10, 60);
  text("Velocidad angular: " + (TWO_PI/period).toFixed(3), 10, 80);
}

function keyPressed(){

  if(key === 'q') amplitude += 10;
  if(key === 'a') amplitude -= 10;

  if(key === 'w') period += 10;
  if(key === 's') period -= 10;

  if(key === 'e') phase += PI/16;
  if(key === 'd') phase -= PI/16;

}
```

![u4a6.gif](attachment:716288fc-7222-44f1-8537-1b5741815505:u4a6.gif)

### Actividad 07

Realmente no hay mayor cambio entre la aplicación de aleatorieidad y fuerzas entre coordenadas cartesianas y polares, lo único que varía es que ahora nuestra posición no es (x, y) sino que es (r*cosθ, r*senθ)

### Actividad 08

Básicamente, en cada frame estoy dibujando 24 círculos repartidos uniformemente por la pantalla pero a cada uno le incremento un poco como el momento de la onda seno en el que está, como 24 ondas desfasadas, pero como están distribuidas uniformemente y el cambio de fase es constante se ve como si fuera una sola onda de seno.

### Actividad 09

Lo que necesito para hacer que ahora tenga dos resortes es volver a crear un objeto que se comporte exactamente igual, pero el único cambio es que por default el objeto de resorte tiene su ancla en la parte superior de la pantalla, que funciona para el primero, pero para el segundo resorte debo modificarlo y decir que el nuevo pivote es el centro de la masa del primer resorte, para que estén encadenados y el segundo resorte se mueva dependiendo del primero.

### Actividad 10

Igual que en el ejemplo de resortes, este sistema tiene como default el spawn point de la cuerda en un punto superior de la pantalla y necesitamos crear un nuevo péndulo que tenga su pivote en el centro de la masa del péndulo anterior.

## Bitácora de aplicación 

### Actividad 11

EL concepto de esta unidad que quise elegir para mi obra fue el del movimiento de la función sinusoide, ya que es una función que se encuentra en la naturaleza y quiero continuar mi temática del curso de obras inspiradas por seres vivos. Hice entonces una mariposa, sus alas moviéndose con una función sinusoide, y además agregando una fuerza circular para que la mariposa no vuele solo en línea recta sino que pueda moverse más orgánicamente haciendo curvas. De la unidad 1 apliqué el concepto de ruido para que el movimiento en general sea suave y randomGaussian para las partículas de polen. De la unidad 2 claramente utilicé el motion101 y el uso de vectores. De la 3 tomé las fuerzas externas, haciendo que cuando se hace click en la pantalla se crea un punto de atracción para la mariposa.

```jsx
let butterfly;
let target = null;

let pollen = [];

function setup() {
  createCanvas(750, 590);
  butterfly = new Butterfly();
}

function draw() {
  background(201,201,241);

  // POLEN
  for(let i = pollen.length-1; i >= 0; i--){
    pollen[i].life -= 2;

    fill(251, 255, 217,pollen[i].life);
    noStroke();
    circle(pollen[i].x,pollen[i].y,4);

    if(pollen[i].life <= 0){
      pollen.splice(i,1);
    }
  }

  butterfly.update();
  butterfly.drag();
  butterfly.show();

  // dibujar objetivo
  if(target){
    translate(target.x,target.y);
    drawFlower()
  }
}

function mousePressed(){

  if(!butterfly.clicked(mouseX,mouseY)){
    target = createVector(mouseX,mouseY);
  }

}

function mouseReleased(){
  butterfly.stopDragging();
}

class Butterfly{

  constructor(){

    // MOTION 101
    this.position = createVector(width/2,height/2);
    this.velocity = createVector(0,0);
    this.acceleration = createVector(0,0);

    this.topspeed = 4;

    // noise
    this.xoff = random(1000);
    this.yoff = random(2000);

    // movimiento circular
    this.angle = random(TWO_PI);

    // drag
    this.dragging = false;
  }

  applyForce(force){
    this.acceleration.add(force);
  }

  update(){

    if(!this.dragging){

      // NOISE FORCE
      let fx = map(noise(this.xoff),0,1,-0.15,0.15);
      let fy = map(noise(this.yoff),0,1,-0.15,0.15);
      let noiseForce = createVector(fx,fy);

      // FUERZA CIRCULAR
      let circleForce = createVector(cos(this.angle),sin(this.angle));
      circleForce.mult(0.05);

      this.applyForce(noiseForce);
      this.applyForce(circleForce);

      // ATRACCIÓN AL CLICK
      if(target){

        let force = p5.Vector.sub(target,this.position);

        let distance = force.mag();
        distance = constrain(distance,20,200);

        force.normalize();

        let strength = 10/distance;

        force.mult(strength);

        this.applyForce(force);
      }

      // BORDES (fuerza de regreso)
      let margin = 60;
      let turn = 0.5;

      if(this.position.x < margin){
        this.applyForce(createVector(turn,0));
      }

      if(this.position.x > width-margin){
        this.applyForce(createVector(-turn,0));
      }

      if(this.position.y < margin){
        this.applyForce(createVector(0,turn));
      }

      if(this.position.y > height-margin){
        this.applyForce(createVector(0,-turn));
      }

      // MOTION101
      this.velocity.add(this.acceleration);
      this.velocity.limit(this.topspeed);
      this.position.add(this.velocity);

      this.acceleration.mult(0);

      this.angle += 0.03;

      this.xoff += 0.01;
      this.yoff += 0.01;

      // POLEN
      pollen.push({
        x:this.position.x + randomGaussian(-3,3),
        y:this.position.y + randomGaussian(-3,3),
        life:255
      });

    }
  }

  show(){

    push();

    translate(this.position.x,this.position.y);

    // orientar hacia movimiento
    let theta = this.velocity.heading();
    rotate(theta + PI/2);

    noStroke();

    if(this.dragging){
      fill('rgb(255,156,209)');
    }else{
      fill(255, 74, 181);
    }

    let flap = sin(frameCount*0.4)*10;

    // ALA DERECHA
    push()
    translate(16,-7)

      push()
      rotate(radians(40+flap))
      ellipse(0,0,25,40)
      pop()

      rotate(radians(-30-flap*0.2))
      ellipse(-15,15,15,24)

    pop()

    // ALA IZQUIERDA
    push()
    translate(-16,-7)

      push()
      rotate(radians(-40-flap))
      ellipse(0,0,25,40)
      pop()

      rotate(radians(30+flap*0.2))
      ellipse(15,15,15,24)

    pop()

    // CUERPO
    fill(50)
    rect(-2,-5,4,20)

    pop()

  }

  clicked(mx,my){

    let d = dist(mx,my,this.position.x,this.position.y);

    if(d < 30){
      this.dragging = true;
      return true;
    }

    return false;
  }

  stopDragging(){
    this.dragging = false;
  }

  drag(){

    if(this.dragging){
      this.position.x = mouseX;
      this.position.y = mouseY;
      this.velocity.mult(0);
    }

  }

}

function drawFlower() {
  noStroke();

    push();

    let petalCount = 6;
    let r = 6;

    fill('#F074D5');
    for (let i = 0; i < petalCount; i++) {
      let angle = TWO_PI / petalCount * i;
      let px = cos(angle) * r;
      let py = sin(angle) * r;
      ellipse(px, py, 10, 10);
    }

    // centro
    fill('#FFEB3B');
    ellipse(0, 0, 6, 6);

    pop();
  
}
```



[LINK AL PROYECTO](https://editor.p5js.org/elennc/full/PU73WA43t)

## Bitácora de reflexión

