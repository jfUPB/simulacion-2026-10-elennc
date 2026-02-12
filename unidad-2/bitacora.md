# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 01

*Distruta esta actividad como quieras. Busca inspiración. Te deseo que te enamores del tema, es irresistible. Te pediré que me cuentes qué trabajo te gustó más y por qué.*

El trabajo que más me gustó es el que parece una arañita, “Crab People” de Raven Kwok. Me gusta mucho cuando algo virtual parece vivo e interactúa con el usuario, y en este proyecto es como si tuvieras una arañita de mascota que baila y sigue tu cursor. Es extremadamente creativo y satisfactiorio.

![u2a1](https://github.com/user-attachments/assets/adeecd4b-0142-4eea-ac58-89bd3b1a0628)

### Actividad 02

1. *¿Cómo funciona la suma dos vectores en p5.js?*
2. *¿Por qué esta línea position = position + velocity; no funciona?*

Un vector está compuesto de componentes, y dos vectores se suman sumando componente a componente, es decir, x1 con x2 y y1 con y2, etc. Por esta razón es que la línea `position = position + velocity` no funciona, ya que no podemos simplemente sumar dos vectores como objetos sino que debemos separar sus componentes y sumarlas independientemente con los equivalentes de cada vector, sumando `*position.x = position.x + velocity.x*` y `*position.y = position.y +velocity.y` .*

En caso de querer sumar vectores sin separar sus componentes, debemos utilizar la expresión `position = p5.Vector.add(position, velocity)` que retorna un nuevo vector que es la suma de position y velocity.

### Actividad 03

1. *¿Qué tuviste que hacer para hacer la conversión propuesta?*
2. *Escribe el código que utilizaste para resolver el ejercicio.*

Para convertir el walker base a uno qe utilice vectores lo único que tengo que hacer es pensar en la posición como un vector en vez de simplemente una variable x y una variable y que se convierten en coordenadas. Lo que hice fue definir en el constructor del walker su posición como un vector de componentes x y y en vez de tener dos variables independientes. Donde en el código original me refería a los atributos x y y del walker ahora me voy a referir a ellas como las componentes x y y del atributo position.

```jsx
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
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}

```

### Actividad 04

1. *¿Qué resultado esperas obtener en el programa anterior?*
2. *¿Qué resultado obtuviste?*
3. *Recuerda los conceptos de paso por valor y paso por referencia en programación.*
4. *¿Qué tipo de paso se está realizando en el código?*
5. *¿Qué aprendiste?*

Analisando el código sin haberle dado play: se crea un vector con componentes (6, 9) y después este mismo se imprime en consola como un string. Después la función playingVector( ) recibe nuestro vector y sobreescribe sus componentes a (20, 30), y nuevamente esto se imprime en consola. Además en la función draw( ) hacemos que se imprima “Only once”, así que al dar play la consola debería decir lo siguiente:

```jsx
(6,9)
(20, 30)
Only once
```

Al darle play, ocurrió casi exactamente lo que deduje del análisis del código, siendo la única diferencia la sintáxis de la consola al imprimir vectores como string.

El paso por referencia es cuando se sobreescribe un parámetro al pasarlo a una función, y el paso por valor es cuando se crea una copia de cierto parámetro para que el original no sea modificado al pasarlo a una función. En este caso estamos utilizando paso por referencia, ya que directamente se le está pasando el vector a la función playingVector( ) que reemplaza los valores originales de éste.

Con este ejercicio aprendemos cómo proteger el parámetro original en caso de que se tenga que referenciar en varias instancias diferentes para no cambiarlo por accidente al pasarlo a una función con un paso de valor, almacenando una copia del valor en una variable diferente. O en caso de que se quiera editar el parámetro bajo ciertas condiciones, también sabemos cómo hacer un paso de referencia.

### Actividad 05

1. *¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?*
2. *¿Para qué sirve el método normalize()?*
3. *Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?*
4. *El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?*
5. *Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.*
6. *¿Para que te puede servir el método dist()?*
7. *¿Para qué sirven los métodos normalize() y limit()?*

- mag( ) calcula la magnitud de un vector, entendido en geometría como la resta de posición final menos posición inicial elevado al cuadrado. magSq( ) es el cuadrado de mag( ), o sea, solo la resta de final menos inicial. A la hora de comparar magnitudes magSq( ) es más eficiente ya que es una menor cantidad de operaciones y la diferencia de magnitud entre dos vectores es directamente proporcional a la diferencia entre sus cuadrados, así que se puede reemplazar tranquilamente.
- El método normalize( ) escala los componentes de un vector para convertirlo en un vector unitario pero conservar su dirección.
- El método dot( ) es el producto punto entre dos vectores, que geométricamente se interpreta como el tamaño de la proyección de un vector sobre otro, o como la sombra que genera un vector sobre otro.
- Hay dos formas de escribir la función dot( ): la versión default, por así decirlo, que es p5.Vector.dot(v1, v2), y la versión estática que es v1.dot(v2), pero ambas crean un vector nuevo producto punto de v1 y v2. La diferencia es solo la forma de escribirse y que en la segunda forma v1 es la “base” de la operación, pero el resultado es exactamente el mismo y depende más de claridad del código.
- El producto cruz geométricamente se toma como un vector perpendicular a los vectores operados, y la dirección se puede definir con la regla de la mano derecha en la que el dedo índice es el vector A, el dedo del medio es B y el pulgar es el producto crus AxB. La magnitud del vector resultante depende del ángulo x entre A y B, remplazando en la expresión |A||B|senx y representa el área del paralelogramo formado por A y B.
- dist( ) calcula la distancia entre dos vectores, restando componente a componente. Me puede servir para condicionales del estilo de “si este objeto está cerca de este, hacer esto.”
- normalize( ) convierte un vector en unitario conservando la dirección, que puedo usar cuando no es necesario tener la magnitud de un vector para optimizar, y limit( ) limita la magnitud de un vector a un número finito, evitando velocidades muy altas o animaciones descontroladas por ejemplo.

### Actividad 06

1. *El código que genera el resultado que te pedí.*
2. *¿Cómo funciona lerp() y lerpColor().*
3. *¿Cómo se dibuja una flecha usando drawArrow()?*

Este es el código inicial:

```jsx
function setup() {
    createCanvas(100, 100);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(30, 0);
    let v2 = createVector(0, 30);
    let v3 = p5.Vector.lerp(v1, v2, 0.5);
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, 'purple');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

Lo primero que hice para lograr que la fecha morada se moviera entre las otras dos fue crear un vector nuevo v3 que es el que va a dar la posición de la punta de la flecha que cambia de rosa a azul. Utiliza la función lerp( ) que calcula valores entre v1 (el final de la flecha rosa) y v2 (El final de la flecha azul), entendiendo que si la variable interP es 0 la flecha morada está directamente encima de la rosa, y si es 1 la flecha está sobre la azul. 

La variable interP aumenta por 0.01 en cada loop, que es el intervalo del lerp, y entonces cada número entre 0 y 1 corresponde a una posición entre la dos puntas de las flechas rosa y azul. Si interP=1, la variable de la dirección del movimiento direction cambia de sentido y comienza a disminuir hasta llegar a 0, donde vuelve a invertirse.

Utilizando la misma variable interP para interpolar el color de la flecha morada, y que cada posición de la flecha sea un color único en el espectro de la mezcla rojo-azul, utilizamos la función lerpColor( ) que hace esencialmente lo mismo que lerp( ) pero en vez de interpolar valores escalares, interpola valores RGB entre rosa y azul. Ya que la flecha inicia estando encima de la rosa, el RGB correspondiente al rosa es el primer valor en la interpolación, y el RGB correspondiente al azul es el último.

Utilizando este vector v4 como el punto final de la flecha morada y el vector v0 como el punto inicial, dibujamos la flecha con su respectivo lerpColor( ).

```jsx
		let v3 = p5.Vector.lerp(v1, v2, interP); // lerp(A,B,X) es como ir pasando de A a B en incrementos de X
  
    drawArrow(v0, v3, lerpColor('rgb(243,81,147)','rgb(124,124,234)',interP)); // 0 es rosa, 1 es azul. 
   
  
	  interP=interP+direction*0.01; // pasa de 0 a 1 muy lento, de rosa a azul, empieza con dirección positiva
	  if(interP>=1) direction =-1; // si es 1 (azul puro) entonces cambia la dirección y empieza a restar hasta llegar a 0
	  else if(interP<=0) direction =1; // si llega a 0 cambia la dirección y vuelve a sumar hasta llegar a 1
```

El siguiente paso sería la línea verde que conecta la punta de la flecha rosa y la azul trazando el camino por el que pasará la punta de la flecha de interpolación. Para ello necesitamos un cuarto vector v4 que indicará el punto final de la flecha rosa, ya que este va a ser nuestro nuevo origen respectivo a esta flecha, y un quinto vector v5 que indica el final de la flecha con respecto a nuestro nuevo origen.

Para asegurarme de que la flecha verde quede encima de ninguna otra, es la primera que se dibuja.

```jsx
		let v4 = createVector(530,50); // el punto final de la flecha roja para tener el sistema de coordenadas desde aqui
    let v5 = createVector(-480, 480); // como el nuevo 0,0 es v4 entonces se mueve en direccion horizontal negativa
  
  
    drawArrow(v4, v5, 'rgb(54,209,67)');
   
```

**Cómo funciona drawArrow( )**

drawArrow( ) inicia con un push( ) y termina en pop( ), lo que garantiza que las flechas no se afecten entre sí sino que cada una esté contenida e individual. Después, se define el grosor y el color con stroke( ), strokeWeight( ) y fill( ). Definimos la base del vector, o sea el punto de inicio con translate(base.x, base.y) y se traza una línea desde el nuevo origen, que es la base o punto de inicio, hasta el punto final vec dado en los parámetros de la función. rotate(vec.heading( )) rota el sistema de coordenadas para estar alineado con el vector y poder dibujar un triángulo apropiado como punta, que se forma con triangle( ) tomando el tamaño como arrowSize (que por default se inicializó como 7) y después trasladándolo o moviéndolo a la punta de la línea que creamos, quedando con la punta superior en toda la punta de la línea.

```jsx
function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(5);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

El código final queda así:

```jsx
let direction=1; // sentido de las manecillas
let interP=0; // inicia en rosa para moverse en sentido de las manecillas

function setup() {
    createCanvas(560, 560);
}

function draw() {
    background('rgb(239,231,197)');

    let v0 = createVector(50, 50);
    let v1 = createVector(480, 0);
    let v2 = createVector(0, 480);
    let v3 = p5.Vector.lerp(v1, v2, interP); // lerp(A,B,X) es como ir pasando de A a B en incrementos de X
    let v4 = createVector(530,50); // el punto final de la flecha roja para tener el sistema de coordenadas desde aqui
    let v5 = createVector(-480, 480); // como el nuevo 0,0 es v4 entonces se mueve en direccion horizontal negativa
  
  
    drawArrow(v4, v5, 'rgb(54,209,67)');
    drawArrow(v0, v1, 'rgb(243,81,147)');
    drawArrow(v0, v2, 'rgb(124,124,234)');
    drawArrow(v0, v3, lerpColor('rgb(243,81,147)','rgb(124,124,234)',interP)); // 0 es rosa, 1 es azul. 
   
  
  interP=interP+direction*0.01; // pasa de 0 a 1 muy lento, de rosa a azul, empieza con dirección positiva
  if(interP>=1) direction =-1; // si es 1 (azul puro) entonces cambia la dirección y empieza a restar hasta llegar a 0
  else if(interP<=0) direction =1; // si llega a 0 cambia la dirección y vuelve a sumar hasta llegar a 1
  
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(5);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

![u2a6](https://github.com/user-attachments/assets/60eb4da9-c09d-4595-9cb5-08397fef5781)

La función lerp( ) y lerpColor( ) son funciones de interpolación, es decir, que encuentran un valor que se encuentra entre dos otros valores. lerp( ) recibe 3 parámetros: A, el punto de inicio de interpolación, B, el punto final, y X, el punto entre A y B en el que me encuentro en este momento. lerp( ) asignará la posición 0 a A y la posición 1 a B, y utilizando un contador que sube de poco a poco (como interP en mi ejericio), se recorre el camino de A a B.

### Actividad 07

1. *Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.*
2. *¿Cómo se aplica motion 101 en el ejemplo?*

### Actividad 08

*Para investigador el significado de esta frase te propone que construyas un experimento donde analices cómo se comporta un objeto en movimiento con:*

- *Aceleración constante.*
- *Aceleración aleatoria.*
- *Aceleración hacia el mouse.*

*1. ¿Qué observaste cuando usas cada una de las aceleraciones propuestas?*

1. *1. ¿Qué observaste cuando usas cada una de las aceleraciones propuestas?*



## Bitácora de aplicación 
### Actividad 09
1. *Describe el concepto de **tu obra generativa.** Explica el concepto de tu obra generativa, qué regla aplicaste para la aceleración y por qué, si fue una decisión de diseño, o qué te evoca, si fue una exploración artística.*
2. *El código de la aplicación.*
3. *Un enlace al proyecto en el editor de p5.js.*
4. *Selecciona capturas de pantalla representativas de tu pieza de arte generativa.*

## Bitácora de reflexión
### Actividad 10
*Enriquecido con esta información, te perdirá que crees algo inspirado en:*

1. *Las ideas de Jared y Jeffrey*
2. *En lo que trabajaste en esta unidad sobre vectores y motion 101.*

*En tu bitácora de aprendizaje:
1. Describe el concepto de **tu obra generativa.** Explica el concepto de tu obra generativa.
2. El código de la aplicación.
3. Un enlace al proyecto en el editor de p5.js.
4. Selecciona capturas de pantalla representativas de tu pieza de arte generativa.*

1. *1. Describe el concepto de **tu obra generativa.** Explica el concepto de tu obra generativa.*
2. *2. El código de la aplicación.*
3. *3. Un enlace al proyecto en el editor de p5.js.*
4. *4. Selecciona capturas de pantalla representativas de tu pieza de arte generativa.*
