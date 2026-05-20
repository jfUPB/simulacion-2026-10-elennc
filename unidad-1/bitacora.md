# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 01

*Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.*

La alatoriedad le da al artista opciones de las cuales puede escoger la mejor forma de comunicar lo que se desea.

### **Actividad 02**

*Realiza el siguiente experimento y reporta los resultados en tu bitácora:*

- *Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.*
- *Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.*
- *Ejecuta el código y escribe en tu bitácora qué sucedió realmente.*
- *Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?*

Quise hacer que el código constantemente evitara repetir pasos, es decir, si en su último movimiento se movió a la derecha, no puede volver a moverse a la derecha inmediatamente después. Esperaría que no haya ninguna línea recta en el camino que dibuja el walker, puesto que una línea es una consecuencia de pasos (2 o más) que van en la misma dirección. 

Este es el código original:

```jsx
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

Aunque hice un cambio por razones estéticas y porque es algo que ya entendía antes de empezar el ejercicio, cambié el color del rastro del walker de negro (0) a un tono morado (126, 31, 161) entendiendo que estamos usando un sistema RGB. :)

```jsx
stroke([126, 31, 161]);
```

El primer cambio que hice al comportamiento del código fue agregar una propiedad a nuestra clase de Walker() para que este tipo de objetos pueda almacenar cuál fue su última decisión (***lastChoice***). Por default, un objeto del tipo Walker se crea sin nada almacenado en su propiedad de última decisión tomada.

```jsx
class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;

    this.lastChoice=null; // no hay last choice si acaba de empezar,,

  }
```

Después, el Walker empieza a moverse creando una variable ***choice*** y debe constantemente comparar el valor de la última decisión tomada con la decisión actual, por lo que implemento un ciclo que solo se libera cuando las dos decisiones tienen diferentes valores; es decir, cuando no se repite la decisión anterior.

Al verificar que no se está repitiendo el último movimiento, el Walker se mueve en dicha dirección utilizando el condicional con las pautas requeridas.

Por último, nuestra decisión de este ciclo (***choice***) pasa a ser nuesta última decisión (***lastChoice***) para el siguiente.

```jsx
step() {
    let choice; 

    do{
      choice = floor(random(4));
    } while (choice == this.lastChoice) // escoge dirección hasta que sea diferente a la última. en el primer paso solo hace esto una vez xq se inició vacía
    
    
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
    
    this.lastChoice=choice; //almaceno el nuevo paso más reciente en la variable para utilizarlo en el siguiente ciclo
    
  }
}
```

Sí ocurrió lo que esperaba: si no hay pasos consecutivos en la misma dirección, no hay líneas rectas en el recorrido del Walker, por lo que su rastro es mucho más compacto y pareciera que intenta colorear en vez de caminar. Su movimiento tiene menos alcance ya que la forma más rápida de recorrer distancias es yendo en línea recta, lo que está prohibido, así que parece no “abrirse” tanto sino recorrer distancia cercana al punto de inicio.

![u1a2](https://github.com/user-attachments/assets/6e6a9cb3-565d-470d-9cbb-a59478380a06)

### Actividad 03

- *En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.*
- *Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.*

La distribución uniforme es aquella que no tiene una tendencia, es decir, todos los valores posibles tienen la misma probabilidad de ser elegidos. En la distribución no uniforme hay una media a la que tienden los valores, se agrupan a su alrededor y entre más cercano un valor es a la media, mayor probabilidad tiene de suceder, mientras que si está más alejado su probabilidad disminuye.

Para hacer que el Walker favoreciera el movimiento hacia la izquierda debemos tener presente que en una distribución no uniforme los valores tienden a una media, por lo que si condicionamos al código a que cuando el valor esté en cierto rango cercano a la media se escoja una dirección específica, éste será el movimiento favorecido. Además, los valores que quedan fuera del rango condicional los dividimos entre las otras 3 opciones de movimiento, reduciendo aún más la posibilidad de que sean escogidas antes que la que queremos nosotros (en este caso la derecha). Estos cambios fueron hechos en la función step()

```jsx
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

    this.lastChoice=null; // no hay last choice si acaba de empezar,,

  }

  show() {
    stroke([126, 31, 161]);
    point(this.x, this.y);
  }

  step() {
    let choice; 
      do {
      const gaussian = randomGaussian(); // sin parámetros, media es 0 y la desviación es 1 por default

      if (gaussian > 0) { // sesgo hacia la derecha
        choice = 0; // derecha
      } else { // otras direcciones
        const others = [1, 2, 3]; // izquierda, abajo, arriba
        choice = random(others);
      }
    } while (choice == this.lastChoice) // escoge dirección hasta que sea diferente a la última. en el primer paso solo hace esto una vez xq se inició vacía
    
    
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
    
    this.lastChoice=choice; //almaceno el nuevo paso más reciente en la variable para utilizarlo en el siguiente ciclo
    
  }
}
```
![u1a3](https://github.com/user-attachments/assets/e13aaa16-dd27-4ea2-9a49-63a14fd18e4c)

[Link a proyecto de p5.js](https://editor.p5js.org/elennc/sketches/0yg5ZpLZE)

### Actividad 04

*Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.*

- *Crea un nuevo sketch en p5.js que represente una distribución normal.*
- *Copia el código en tu bitácora.*
- *Coloca en enlace a tu sketch en p5.js en tu bitácora.*
- *Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.*

Lo que hice fue utilizar el ejemplo del punto 3 como base, pero modificándolo para que los puntos en vez de hacer una línea se distribuyeran por la pantalla, tendiendo a acomodarse en el centro usando randomGaussian(). Utilicé la función dist() de p5js para tomar la distancia entre el punto y el centro del canvas (200, 200), y además hice que entre más cerca esté el punto del centro, más grande sea el punto. La opacidad es de 80% para que se note mejor la acumulación por la tendencia al centro causada por el randomGaussian().

```jsx
function setup() {
  createCanvas(400, 400);

  background(255);
}

function draw() {
  // Style the circles.
  noStroke();
  

  // Gaussian distribution with a mean of 50 and sd of 1.
  x = randomGaussian(200, 40);
  y = randomGaussian(200, 40);

   // Distancia al centro
  let distance = dist(x, y, 200, 200);

  // cerca = grande, lejos = pequeño
  let diameter = map(distance, 0, 100, 8, 2, true);

  let color = floor(random(5));
  
switch (color) {
    case 0:
      fill(68, 48, 37, 100);
      break;
    case 1:
      fill(127, 89, 53, 100);
      break;
    case 2:
      fill(171, 127, 102, 100);
      break;
    case 3:
      fill(236, 156, 157, 100);
      break;
    case 4:
      fill(242, 207, 203, 100);
      break;
    }
    circle(x, y, diameter);
}

```
También quise hacer que cada punto sea de un color aleatorio de un total de 5, para lo que usé un switch-case.

[**Link a proyecto de p5.js**](https://editor.p5js.org/elennc/sketches/yWX1rtb-v)

<img width="496" height="495" alt="image" src="https://github.com/user-attachments/assets/de31f330-e6d8-4a1e-b8da-d788033b885e" />

![u1a4](https://github.com/user-attachments/assets/2b17080b-c635-4cde-a6cf-0a3855c1c06f)


### Actividad 05

- *Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.*
- *Explica por qué usaste esta técnica y qué resultados esperabas obtener.*
- *Copia el código en tu bitácora.*
- *Coloca en enlace a tu sketch en p5.js en tu bitácora.*
- *Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.*

```jsx
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(740, 560);
  walker = new Walker();
  background(206, 184, 214);
}

function draw() {
  walker.step();
  walker.show();
}

function levyFlight() {
  while (true) {
    let step = random(0, 15);   // candidato (tamaño del paso)
    let probability = random(0, 1);

    // cuanto más pequeño el paso, más probable que sea aceptado
    if (probability < 1 / (step + 1)) {
      return step;
    }
  }
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;

    this.lastChoice=null; // no hay last choice si acaba de empezar,,

  }

  show() {
    //stroke([126, 31, 161]);
    //point(this.x, this.y);
    fill(126, 31, 161);
    circle(this.x, this.y, 4);
  }
  
  

  step() {
    let choice;
    let stepSize = levyFlight();

    do{
      choice = floor(random(4));
    } while (choice == this.lastChoice) // escoge dirección hasta que sea diferente a la última. en el primer paso solo hace esto una vez xq se inició vacía
    
    
    if (choice == 0) {
      this.x += stepSize;
    } else if (choice == 1) {
      this.x -= stepSize;
    } else if (choice == 2) {
      this.y += stepSize;
    } else {
      this.y -= stepSize;
    }

    this.lastChoice = choice; //almaceno el nuevo paso más reciente en la variable para utilizarlo en el siguiente ciclo

  }
}
```

Utilicé nuevamente el Walker como base para este punto, adicionándole una función llamada levyFlight() que determina el tamaño del paso que tomará el walker. El tamaño de un paso está en el rango de 0 a 15, y se compara con la probabilidad utilizando la ecuación 1/(step+1). Escogimos esta función porque es inversamente proporcional al paso pero que nunca llegue a ser 0.

 Si al remplazar el valor del paso en la ecuación anterior el resultado es mayor a la probabilidad escogida al azar, el programa devuelve el valor del paso para poder utilizarlo en la función de step() del walker. Así asegurammos que los pasos cortos sigan siendo más probables que los pasos cortos.


```jsx
function levyFlight() {
  while (true) {
    let step = random(0, 15);   // candidato (tamaño del paso)
    let probability = random(0, 1);

    // cuanto más pequeño el paso, más probable que sea aceptado
    if (probability < 1 / (step + 1)) {
      return step;
    }
  }
}
```

Ahora en vez de sumar una unidad en cualquier dirección que es escogida sumamos la cantidad que se determinó para el siguiente paso. Así queda la función de step:
```jsx
step() {
    let choice;
    let stepSize = levyFlight();

    do{
      choice = floor(random(4));
    } while (choice == this.lastChoice) // escoge dirección hasta que sea diferente a la última. en el primer paso solo hace esto una vez xq se inició vacía
    
    
    if (choice == 0) {
      this.x += stepSize;
    } else if (choice == 1) {
      this.x -= stepSize;
    } else if (choice == 2) {
      this.y += stepSize;
    } else {
      this.y -= stepSize;
    }

    this.lastChoice = choice; //almaceno el nuevo paso más reciente en la variable para utilizarlo en el siguiente ciclo

  }
```

Viendo que el Walker original no recorre gran parte de la pantalla esperaba notar que ahora parecía saltar de un lado a otro moviéndose como si buscara comida, como describe la guía, y esto es exactamente lo que pasó. Ahora tiene más área de recorridp gracias a que reduce la posibilidad de que vuelva a pisar lugares donde ya estuvo antes.
![u1a5](https://github.com/user-attachments/assets/934696e0-034a-41f5-9f62-7dffec3ffb00)

[**Link a proyecto de p5.js**](https://editor.p5js.org/elennc/sketches/SlIKgbUGm)

## Bitácora de aplicación 
### Actividad 07

*Vas a crear una obra generativa interactiva en tiempo real utilizando los conceptos de aleatoriedad que has aprendido en esta unidad.*

*Tu obra debe:*

- *Usar al menos tres conceptos estudiados en esta unidad COMBINADOS de manera creativa y coherente.*
- *Tu obra de ser interactiva y generativa en tiempo real. Puedes usar el mouse, el teclado o cualquier otro sensor de entrada para interactuar con la obra.*

Incluir:

- *Un texto donde expliques el concepto de obra generativa.*
- *Copia el código en tu bitácora.*
- *Coloca en enlace a tu sketch en p5.js en tu bitácora.*
- *Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.*

Una obra generativa es una fabricación artística creada a partir de código o un sistema que trabaja por sí solo pero que fue diseñado por un artista. Es decir, es la creación de un producto a partir de conceptos matemáticos que corre un programa con el fin de tener algo estéticamente satisfactorio.

Para mi proyecto, quería que pareciera un animal, ya que me quedó sonando eso de una de las explicaciones del texto guía, entonces utilicé un comportamiento parecido al de un walker normal, incluyendo el Lévy Flight para llegar a ese comportamiento animal que buscaba, que hace que se mueva más en la pantalla por la introducción de la probabilidad de los saltos largos. Como en el ejercicio 5, creé una función llamada levyStep() que determina la longitud de los pasos de mi Walker.

```jsx
function levyFlight() {
  while (true) {
    let step = random(0, 50);   // candidato (tamaño del paso)
    let probability = random(0, 1);

    // cuanto más pequeño el paso, más probable que sea aceptado
    if (probability < 1 / (step + 1)) {
      return step;
    }
  }
}
```

Luego, llamé esta función en mi draw() para tener el tamaño del paso en cada uno de mis loops.

```jsx
let stepSize = levyFlight();
```

Lo siguiente entonces fue hacer que mi Walker tomara el paso con la distancia adecuada, pero sin que saltara de un lugar a otro de forma errática sino suavemente, por lo que implementé un Perlin Noise y una función de map() para determinar las coordenadas del paso, negativas o positivas, y dependiendo del tamaño del step definido por levyFlight(). El movimiento en y está un poco desfazado con respecto a x para darle más naturalidad al asunto. 

```jsx
// posición del cosito,
let dx = map(noise(t), 0, 1, -stepSize, stepSize);
let dy = map(noise(t + 1000), 0, 1, -stepSize, stepSize);
```

Luego está la interacción con el mouse, que siguiendo el concepto de animal, hace que el walker se acerque al mouse ligeramente, sin estar siguiéndolo directamente, como si fuera con curiosidad. Ubicamos un vector desde el Walker hasta el mouse, para tener su dirección, y calculamos la distancia entre ambos. Calculamos la fuerza con la que el mouse afectará al Walker diciendo que entre más lejos esté del mouse, menos será la atracción. Finalmente, calculamos la posición del Walker tomando la posición a la que iría naturalmente sin influencia del mouse (dx, dy) y sumándosela a la posición actual, para después sumar también la dirección de la fuerza de atracción del mouse (mx, my) en cada una de sus componentes, la influencia que esta fuerza tiene en su movimiento (0.025) para que no salte, y finalmente la fuerza misma determinada por la distancia del mouse al Walker.

```jsx
	let mx = mouseX - x; // posición del mouse menos la posición de la bolita
  let my = mouseY - y;

  // distancia de bolita al mouse
  let distance = dist(x, y, mouseX, mouseY);

  // mouse más cerca = más fuerza que lo atrae
  let mouseForce = map(distance, 0, width, 1.5, 0);

  // influencia del mouse
  x += dx + mx * 0.025 * mouseForce; // suma de la posición del cosito y la influencia del mouse
  y += dy + my * 0.025 * mouseForce;
```

Hasta aquí tendría un Walker que tiene un comportamiento diferente a uno básico, y se vería igual que uno, por lo que agregué entonces un pequeño sistema de partículas que siguen a la bolita principal utilizando una distribución Gaussiana. Siendo la media de la distribución 0, las partículas tienden a estar cerca al centro pero se ubican en un radio de 12 en toda dirección. Esta coordenada de la posición de la partícula se suma con la posición del punto principal dentro de un ciclo de 15 rotaciones por posición para que siempre lo siga un grupo de partículas.

```jsx
// partículas, en cada posición 15 partículas 
  for (let i = 0; i < 15; i++) {
    let gx = randomGaussian(0, 12);
    let gy = randomGaussian(0, 12);

    noStroke();
    fill(131,45,81, 35);
    circle(x + gx, y + gy, 7);
  }
```

Ésta es nuestra bolita principal:

```jsx
// bolita principal
fill(234, 105, 147, 100);
circle(x, y, 22);
```

Y el incremento del tiempo por loop para que el ruido siga evolucionando:

```jsx
// tiempo para el ruido
t += 0.01;
```

Por último, un límite para que el bichito no se salga de la pantalla:

```jsx
// para que no se slaga de la pantalla
x = constrain(x, 0, width);
y = constrain(y, 0, height);
```

Éste es el código completo:

```jsx
let x, y;
let t = 0;

function setup() {
  createCanvas(740, 560);
  background(207, 221, 157);

  x = width / 2;
  y = height / 2;
}

function draw() {
  background(207, 221, 157, 15);
  
  let stepSize = levyFlight();

  // posición del cosito,
  let dx = map(noise(t), 0, 1, -stepSize, stepSize);
  let dy = map(noise(t+1000), 0, 1, -stepSize, stepSize);
  
  let mx = mouseX - x; // posición del mouse menos la posición de la bolita
  let my = mouseY - y;

  // distancia de bolita al mouse
  let distance = dist(x, y, mouseX, mouseY);

  // mouse más cerca = más fuerza que lo atrae
  let mouseForce = map(distance, 0, width, 1.5, 0);

  // influencia del mouse
  x += dx + mx * 0.025 * mouseForce; // suma de la posición del cosito y la influencia del mouse
  y += dy + my * 0.025 * mouseForce;

  // partículas, en cada posición 15 partículas 
  for (let i = 0; i < 15; i++) {
    let gx = randomGaussian(0, 12);
    let gy = randomGaussian(0, 12);

    noStroke();
    fill(131,45,81, 35);
    circle(x + gx, y + gy, 7);
  }

  // bolita principal
  fill(234, 105, 147, 100);
  circle(x, y, 22);

  // tiempo para el ruido
  t += 0.01;

  // para que no se slaga de la pantalla
  x = constrain(x, 0, width);
  y = constrain(y, 0, height);
}

function levyFlight() {
  while (true) {
    let step = random(0, 50);   // candidato (tamaño del paso)
    let probability = random(0, 1);

    // cuanto más pequeño el paso, más probable que sea aceptado
    if (probability < 1 / (step + 1)) {
      return step;
    }
  }
}

```

[**Link a proyecto de p5.js**](https://editor.p5js.org/elennc/sketches/3jtzuYy7o)

![u1a7](https://github.com/user-attachments/assets/79052eed-9f7b-4bbd-b3a5-2a5313a18236)



## Bitácora de reflexión

### Actividad 08

*En tu bitácora de aprendizaje. Responde con tus propias palabras a las siguientes preguntas.*

1. *Describe la diferencia fundamental entre la aleatoriedad generada por `random()` y la apariencia de aleatoriedad del Ruido Perlin (`noise()`). ¿En qué tipo de situación usarías cada una?*
    
    Los valores generados por random( ) no siguen ningún patrón en específico, mientras que las de el Ruido Perlin son cercanas una de la otra, sin ser seguidas o predecibles. Random se usa cuando se quiere obtener un movimiento errático y totalmente descontrolado, mientras que el ruido actúa como un suavizante en el comportamiento del código.
    
2. *Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?*
    
    Una caminata con distribución uniforme no tiene tendencias y todos los caminos son igualmente probables, por lo que es muy simple y totalmente impredecible. Una caminata con distribución Gaussiana o normal tiene una tendencia en su comportamiento, por lo que es más fácil predecir sus siguientes pasos o al menos hacerse una idea de cuál será su recorrido.
    
3. *¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple*
    
    La aleatoriedad lo es todo en el arte generativo, pues es la encargada de generar suficientes posibilidades para que el artista pueda visualizar su idea y pulirla poco a poco (si desea) limitando más y más el programa.
    
4. *Piensa en tu obra final (Actividad 07). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.*
    
    Creo que el concepto más importante en mi trabajo fue el del Lévy Flight, pues es el que da ese comportamiento natural de “estar buscando algo” que permite recorrer mayor cantidad de la pantalla y hace que todo se vea más natural.
    
5. *¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?*
    
    Una caminata es un código que dibuja una secuencia de puntos, cada uno siendo un paso desde el anterior, escogiendo aleatoriamente entre arriba, abajo, derecha o izquierda para terminar con un trazo totalmente único y hecho por azar. Las caminatas con Lévy Flight son las que tienen una posibilidad, por muy pequeñas que sean, de dar un paso de mayor distancia al default (que hace una línea continua). Esta posibilidad permite que el Walker recorra mayor área de la pantalla.





