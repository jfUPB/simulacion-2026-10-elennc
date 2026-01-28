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


### Actividad 04

*Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.*

- *Crea un nuevo sketch en p5.js que represente una distribución normal.*
- *Copia el código en tu bitácora.*
- *Coloca en enlace a tu sketch en p5.js en tu bitácora.*
- *Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.*

```jsx
function setup() {
  createCanvas(400, 400);

  background([234,157,174]);
}

function draw() {
  noStroke();
  
  x = randomGaussian(200, 40);
  y = randomGaussian(200, 40);

   // Distancia al centro
  let distance = dist(x, y, 200, 200);

  // cerca = grande, lejos = pequeño
  let diameter = map(distance, 0, 100, 8, 2);

  // lejos = más transparente
  let alpha = map(distance, 0, 200, 80, 5);

  fill([69,21,27], alpha);
  circle(x, y, diameter);
}
```

[Link a proyecto de p5.js](https://editor.p5js.org/elennc/full/yWX1rtb-v)

## Bitácora de aplicación 



## Bitácora de reflexión
