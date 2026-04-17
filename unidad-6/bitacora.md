# Unidad 6

## Bitácora de proceso de aprendizaje
### Actividad 01

1. *Selecciona dos imágenes o piezas de Tyler Hobbs que te llamen la atención.*
    1. 64 Bits of Sunlight #288
    2. Translated Gestures 
    
<img width="478" height="649" alt="Captura de pantalla 2026-04-15 104301" src="https://github.com/user-attachments/assets/1da627ab-5cf7-4f84-bb7f-0fc91864a809" />
<img width="1717" height="848" alt="Captura de pantalla 2026-04-15 104655" src="https://github.com/user-attachments/assets/53b6d335-571e-4f46-939c-f258114694c4" />
    
2. *Describe qué decisiones visuales reconoces en ellas:*
    - composición,
        1. Líneas verticales que recorren todo el espacio. 
        2. Cada uno de los cuadros tiene un conjunto de “manchas” monocromáticas. Al tener todas las piezas una al lado de la otra logramos la composición completa de Translated Gestures.
    - densidad,
        1. Hay lugares donde las líneas están agrupadas con una densidad muy alta, pero en otros lugares es muy baja. Las líneas mismas están compuestas por una serie de puntos muy pequeños que vistos desde lejos dan la impresión de ser una línea continua.
        2. Diría que son de muy baja densidad, puesto que cada una de las manchas está completamente aparte de la otra, pero individualmente cada mancha está compuesta por múltiples series de puntos que forman líneas para simular una brocha, entonces ahí su densidad es mayor.
    - dirección del movimiento,
        1. Todas las líneas siguen la dirección vertical formando pequeñas ondas. Todas son iguales, pero distribuídas a lo ancho del canvas.
        2. No hay una dirección que todas las manchas tengan, pero la formación de cada mancha depende de una generación con ruido aleatorio de vectores, para que sea suave pero no totalmente predecible.
    - color,
        1. El color varía entre amarillos, naranjas, rojos, blancos, azules y un poco de verde y café. No hay patrones de color.
        2. Cada mancha tiene su color individual y no hay interacción entre ellos.
    - ritmo,
        1. Todas las líneas siguen un mismo patrón ondulante, por lo que resulta una pieza fluída sin tener muchos elementos diferentes.
        2. Cada uno de los canvas tiene su propio ritmo, pero están totalmente independientes. Siguen sus propias reglas pero no hay reglas del conjunto total.
    - repetición y variación.
        1. Hay repetición solo en el movimiento, pues la densidad y el color parece ser completamente aleatorio formando manchas de diferente tamaño e intensidad a lo largo de la pieza. También se repiten los círculos que forman dichas líneas, pero en ellos no hay variación aparte de su posición en el canvas.
        2. No hay repetición más allá de que cada una de las piezas que forma el conjunto tiene el mismo concepto base de “mancha”
3. *Explica por qué esas decisiones te parecen potentes.*
    
    Son estas decisiones las que hacen que las obras se distingan entre sí: ambas utilizan colores y líneas, pero es la forma en la que se controla la dirección, la densidad y el color que hace que sean distinguibles una de la otra y que cada una cuente una historia diferente.
    
4. *Escribe una hipótesis: ¿Qué tipo de reglas o sistema crees que podrían estar detrás de esa pieza?*
    1. Supondría primero que hay un array de círculos que se ubican a lo largo de una curva sinusoidal vertical, que a su vez hace parte de un array de estas curvas idénditcas que las ubica en diferentes coordenadas horizontales del canvas. Aleatoriamente se decide en qué partes no se renderiza un pedazo de la curva, al igual que se decide cambiar el color en cierto rango de coordenadas del canva para crear pequeños grupos de color.
    2. Creo que probablemente los “brochazos” sean algo como un grupo de walkers que se mueven en la misma dirección pero un poco separados uno de otro. Tienden a un punto específico y supondría que toman mucho tiempo para lograr cubrir tanta área del canvas.

### Actividad 02

1. *Explica con tus propias palabras qué es un agente autónomo.*
    
    Es un ser que toma decisiones propias sobre qué hacer, en vez de solo seguir lo impuesto por fuerzas mayores.
    
2. *Explica qué es una `steering force`.*
    
    Son aquellas fuerzas que “empujan” a un objeto a cambiar su trayectoria estipulada naturalmente.
    
3. *Compara una `steering force` con una fuerza externa como la gravedad.*
    
    Las fuerzas de gravedad, fricción y el viento son fuerzas generales que afectan al objeto por consecuencia, mientras que una steering force es una fuerza que afecta al objeto porque este así lo desea. Hace que “salga” de su trayectoria default para alcanzar la dirección deseada.
    
4. *Describe por qué estas ideas son útiles para diseñar comportamiento visual y no solo para simular movimiento.*
    
    El pensar en el objeto como un ser pensante hace que las simulaciones sean más cercanas a la realidad, como en la naturaleza los insectos por ejemplo siguen sus instintos moviéndose tomando en cuenta la consciencia que tienen sobre sí mismos.
    

### Actividad 03

1. *¿Cómo está construido el campo de flujo?*
    
    Un campo de flujo es una cuadrícula de direcciones que el objeto toma para saber por dónde moverse en el espacio. Puede ser totalmente aleatorio o cambiar lentamente como con un noise.
    
2. *¿Qué representa cada celda o vector del campo?*
    
    Una dirección deseada del objeto.
    
3. *¿Cómo usa un agente su posición para consultar el campo?*
    
    La cuadrícula determina rangos de coordenadas en el espacio a los que les asigna una dirección específica. El agente toma su posición y dependiendo de en qué rango de la cuadrícula se encuentre, toma una nueva dirección.
    
4. *¿Cómo se convierte el vector consultado en una decisión de movimiento?*
    
    El vector en el que está parado un ajente es la dirección deseada, entonces se le resta a la dirección que ya tiene el agente y logramos así una steering force que hace que el objeto se “desvíe” y cambie de dirección. 
    
5. *Identifica parámetros importantes del sistema, por ejemplo:*
    - resolución: qué tan grande es la cuadrícula, qué tan amplio es ese rango que
    - `maxspeed:` es la magnitud de la velocidad deseada, un limitador para que sea razonable
    - `maxforce:` magnitud de la fuerza de dirección, la limita para que el movimiento sea realista en vez de solo rápido.
    - cantidad de agentes: es una decisión conceptual, depende de qué se quiere contar con la pieza
6. *Realiza al menos una modificación y analiza el efecto visual que produce.*

*Además, responde:*

- *¿Qué tipo de movimiento produce este algoritmo?*
- *¿Qué sensaciones visuales te sugiere?*
- *¿En qué tipo de pieza musical imaginas que podría funcionar bien?*

### Actividad 04

1. *Explica con tus palabras las tres reglas básicas:*
    - separación: la decisión de evitar chocar con otros agentes del sistema
    - alineación: seguir la dirección de los agentes más cercanos
    - cohesión: evita que se alejen del grupo
2. *Identifica qué parámetros controlan estas reglas.*
    
    La separación depende de la posición y el estar constantemente calculando la posición de los agentes cercanos. Alineación depende del vector de velocidad que tengan los vecinos, y coeción depende de un rango de posición relativo al “centro” del grupo.
    
3. *Modifica uno o más pesos del sistema y describe el efecto visual y colectivo.*
4. *Describe el comportamiento emergente observado:*
    - compacto,
    - disperso,
    - estable,
    - nervioso,
    - caótico,
    - fluido.

*Además, responde:*

- *¿Qué atmósfera visual produce el flocking?*
- *¿En qué tipo de relación con una canción podría funcionar mejor este algoritmo?*

### Actividad 05

|  | **FLOW FIELDS** | **FLOCKING** |
| --- | --- | --- |
| **Tipo de movimiento que producen** | Hace que los ajentes se muevan como un río, pasando uno encima de otro siguiendo las direcciones de la rejilla de vectores. No necesariamente están juntos todos los agentes | Viajan en grupo, como una manada. No pasan por encima unos de otros, se comportan casi como una sola entidad compuesta. |
| **Nivel de control visual** | Hay poco control en el sentido de que como no necesariamente los agentes están cerca unos de los otros, puede parecer más caótico | Considero que hay más control ya que se puede tratar a un grupo de agentes como su fuera un solo ente, ya que siempre se mueven en grupo. |
| **Nivel de emergencia** |  |  |
| **Tipo de atmósfera o sensación** | Cáos, pero a su vez puede representar fluidez. Depende de la velocidad de los agentes. | Es más intimidante, como un grupo al ataque. Es más “abrumador” |
| **Relación posible con una pieza musical** | Los vectores de la cuadrícula podrían cambiar dirección con los altos y bajos de una canción, por ejemplo. | Se podría hacer una pieza en la que el sistema completo sigue un recorrido que “mapea” la música, o por ejemplo hacer algo como que el parámetro de separación aumente con los decibeles. |
| **Ventajas y limitaciones** | Siento que los flow fields tienen un efecto muy lindo de como de “aleatorieidad controlada” logrando distribuir elementos en movimiento por un espacio, pero puede tener limitaciones con respecto a que los agentes pueden acumularse en ciertos puntos y pasar unos sobre otros y puede verse un poco caótico. | El flocking es bueno para sistemas más uniformes, que todos los agentes fluyen juntos y no en direcciones totalmente diferentes, pero puede verse como muy pequeño en un espacio muy grande ya que no abarcan tanto espacio al mantenerse juntos. |

Cierra la actividad respondiendo:

Si quisieras diseñar visuales para una canción contemplativa, agresiva, melancólica o eufórica, ¿Cuál algoritmo usarías en cada caso y por qué?

Para contemplativa tal vez utilizaría un flow field con baja velocidad, que se sienta como agua o un tipo de fluido en toda la pantalla. Agresiva podría ser un flowfield con alta velocidad para incitar cáos, o también un sistema flocking que se mueva muy rápido hacia algo por ejemplo, como un enjambre de abejas. Melancólica probablemente podría hacer un flocking con muchas entidades que se muevan constantemente hacia una dirección específica, que también puede parecer como un mar o algo más tranquilo y pensativo. Para euforia podría combinar ambos, y tener varias manadas que van rápido por un flow field, como un festival de bichitos.

## Bitácora de aplicación 
### Actividad 06

1. ***Concepto visual***
    
    Quiero transmitir la idea de canción de cuna, la magia y los sueños, que sea totalmente onírico. Para esto busco el cambio de color y la emisión de partículas que parecen brillos. Quiero que sea como ondas, como si fuera agua o un líquido brillante que se mueve y reacciona a la música cambiando de color entre azul y rosa. Cuando se hace click se puede cambiar la dirección del movimiento sin perturbar la reacción a la música.
    
    Cuando se alargan notas, el sistema pasa de azul a rosa pero vuelve a la normalidad. También hay estrellas que cambian de tamaño y opacidad dependiendo de la música.
    
2. ***Mapa de decisiones:***
    
    Fondo abstracto tipo “líquido”: quería representar la magia y sueños, entonces me pareció que sería algo que fluye como un líquido pero es como el espacio lleno de cosas pero misterioso. por esto mismo el mouse hace un efecto ripple como pasaría en un líquido real
    
    Cambio de color: el líquido reacciona al sonido como agua en un parlante, como si vibrara, pero en vez de moverse cambia de color como si se “avivara”
    
    Color azul y rosa: son los colores en los que pienso cuando me imagino los sueños. Dan sensación de magia
    
    Estrellas: me parece que son representativas de cosas como los sueños, la magia, la noche, el espacio, etc.
    
    Flow field en las estrellas con baja velocidad: quería que fueran como brillos flotando en el espacio, o en el agua
    
3. ***Mapa de interpretación***
    - click: ripple effect en el líquido. también se ve en el drag
    - click sostenido: afecta el flow field haciendo que las estrellas tiendan a seguir el camino que recorrió
    - flow field: mueve las estrellas por la pantalla con fluidez para dar el efecto de que flotan en el espacio y están en todos lados, pero a su vez están flotando en el líquido y se ven afectadas por corrientes
    - audio: controla el color del líquido, el tamaño de las estrellas y su opacidad.
4. ***Boceto***
    <img width="2360" height="1640" alt="Untitled_Artwork (8)" src="https://github.com/user-attachments/assets/7d287862-f613-4a3b-a311-db9c8329aaed" />

5. ***Moodboard***
    <img width="1920" height="1080" alt="Untitled design" src="https://github.com/user-attachments/assets/098eacea-65bc-43ce-8c0b-3f148f6415eb" />

   
6. ***Código***

```jsx
let song;
let amp;
let fft;

let t = 0;
let scaleFactor = 5;

let pg;

let dragging = false;
let prevMouse;

let stars = [];
let fireflies = [];

// 🌊 ripples persistentes
let ripples = [];

function preload() {
  song = loadSound('Dreamer (Acoustic).mp3');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  pixelDensity(1);
  noCursor();

  pg = createGraphics(width / scaleFactor, height / scaleFactor);
  pg.pixelDensity(1);

  amp = new p5.Amplitude();
  amp.setInput(song);

  fft = new p5.FFT();
  fft.setInput(song);

  for (let i = 0; i < 70; i++) stars.push(new Star());
  for (let i = 0; i < 12; i++) fireflies.push(new Firefly());

  prevMouse = createVector(mouseX, mouseY);
}

function draw() {
  let level = amp.getLevel();
  let bass = fft.getEnergy("bass");

  // 🌊 actualizar ripples
  for (let i = ripples.length - 1; i >= 0; i--) {
    ripples[i].update();
    if (ripples[i].life <= 0) {
      ripples.splice(i, 1);
    }
  }

  // 🌊 AGUA
  pg.loadPixels();

  for (let x = 0; x < pg.width; x++) {
    for (let y = 0; y < pg.height; y++) {

      let nx = x * 0.03;
      let ny = y * 0.03;

      let n = noise(nx, ny + t);
      nx += n * 2;
      ny += n * 2;

      let amplitude = 20 + level * 250;
      let freq = 2 + bass * 0.01;

      let wave =
        sin(nx * freq + t) +
        sin(ny * freq + t * 1.1) +
        sin((nx + ny) * freq * 0.7 + t * 0.8);

      let rippleSum = 0;

      // 🌊 aplicar todos los ripples vivos
      for (let r of ripples) {
        let d = dist(
          x * scaleFactor,
          y * scaleFactor,
          r.pos.x,
          r.pos.y
        );

        let waveFront = d - r.radius;

        let influence = exp(-abs(waveFront) * 0.05);

        rippleSum += sin(waveFront * 0.1 - t * 3) * influence * r.strength * r.life;
      }

      let value = wave * amplitude + rippleSum;

      let smoothVal = Math.tanh(value * 0.02);
      let crest = pow(abs(sin(value * 0.04)), 4);

      let hue = map(level, 0, 0.3, 200, 320);

      let baseR = map(hue, 200, 320, 120, 255);
      let baseG = map(hue, 200, 320, 150, 80);
      let baseB = 255;

      let brightness = map(smoothVal, -1, 1, 0.4, 1);
      let glow = crest * map(level, 0, 0.3, 80, 200);

      let rCol = baseR * brightness + glow;
      let gCol = baseG * brightness + glow * 0.8;
      let bCol = baseB * brightness + glow * 1.2;

      let index = (x + y * pg.width) * 4;

      pg.pixels[index + 0] = constrain(rCol, 0, 255);
      pg.pixels[index + 1] = constrain(gCol, 0, 255);
      pg.pixels[index + 2] = constrain(bCol, 0, 255);
      pg.pixels[index + 3] = 255;
    }
  }

  pg.updatePixels();
  image(pg, 0, 0, width, height);

  // ⭐ estrellas
  for (let s of stars) {
    s.follow();
    s.update(level);
    s.show();
  }

  // ✨ luciérnagas
  blendMode(ADD);
  for (let f of fireflies) {
    f.update(level);
    f.show();
  }
  blendMode(BLEND);

  t += 0.006 + level * 0.1;
  prevMouse.set(mouseX, mouseY);
}

// 🌊 ripple class
class Ripple {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.radius = 0;
    this.life = 1; // 1 → 0
    this.strength = random(80, 140);
  }

  update() {
    this.radius += 4;
    this.life -= 0.02; // duración ~1 segundo
  }
}

// ⭐ STAR (igual que antes)
class Star {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);

    this.baseSize = random(4, 8);
    this.size = this.baseSize;
    this.targetSize = this.baseSize;

    this.baseAlpha = random(20, 50);
    this.alpha = this.baseAlpha;

    this.reactivity = random(0.5, 1.5);
  }

  follow() {
    let scale = 0.0015;

    let angle = noise(
      this.pos.x * scale,
      this.pos.y * scale,
      t * 0.3
    ) * TWO_PI * 2;

    let flow = p5.Vector.fromAngle(angle);
    flow.mult(0.25);

    if (dragging) {
      let mouseDir = p5.Vector.sub(
        createVector(mouseX, mouseY),
        prevMouse
      );

      let d = dist(this.pos.x, this.pos.y, mouseX, mouseY);
      let radius = 120;

      if (d < radius) {
        let falloff = 1 - (d / radius);
        falloff = pow(falloff, 2);

        mouseDir.normalize();
        mouseDir.mult(falloff * 6);

        flow.add(mouseDir);
      }
    }

    this.acc.add(flow);
  }

  update(level) {
    let boosted = pow(level * 4, 1.5);

    let audioSize = map(boosted, 0, 1, 0, 12);
    this.targetSize = this.baseSize + audioSize * this.reactivity;
    this.size = lerp(this.size, this.targetSize, 0.15);

    let audioAlpha = map(boosted, 0, 1, 0, 255);
    this.alpha = lerp(this.alpha, this.baseAlpha + audioAlpha, 0.2);

    this.vel.add(this.acc);
    this.vel.limit(2.5);
    this.pos.add(this.vel);
    this.acc.mult(0);

    // separación
    let separationRadius = 40;
    let steer = createVector(0, 0);
    let count = 0;

    for (let other of stars) {
      let d = dist(this.pos.x, this.pos.y, other.pos.x, other.pos.y);

      if (other !== this && d < separationRadius) {
        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.normalize();
        diff.div(d);
        steer.add(diff);
        count++;
      }
    }

    if (count > 0) {
      steer.div(count);
      steer.mult(0.5);
      this.vel.add(steer);
    }

    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.x > width) this.pos.x = width;
    if (this.pos.y < 0) this.pos.y = height;
    if (this.pos.y > height) this.pos.y = height;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    fill(255, 220, 255, this.alpha);
    noStroke();
    drawStar(0, 0, this.size * 0.5, this.size, 5);
    pop();
  }
}

// ✨ FIREFLY igual
class Firefly {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.offset = random(1000);
    this.size = random(2, 4);
  }

  update(level) {
    let n = noise(this.pos.x * 0.002, this.pos.y * 0.002, t * 0.3 + this.offset);
    let angle = n * TWO_PI;

    this.pos.x += cos(angle) * 0.4;
    this.pos.y += sin(angle) * 0.4;

    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.y < 0) this.pos.y = height;
    if (this.pos.y > height) this.pos.y = 0;

    this.glow = (sin(t * 2 + this.offset) * 0.5 + 0.5);
    this.glow *= map(level, 0, 0.3, 0.8, 2);
  }

  show() {
    noStroke();
    let alpha = this.glow * 255;

    fill(255, 240, 180, alpha * 0.05);
    circle(this.pos.x, this.pos.y, this.size * 20);

    fill(255, 240, 180, alpha * 0.1);
    circle(this.pos.x, this.pos.y, this.size * 10);

    fill(255, 255, 200, alpha);
    circle(this.pos.x, this.pos.y, this.size);
  }
}

function drawStar(x, y, r1, r2, n) {
  let angle = TWO_PI / n;
  let half = angle / 2;

  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    vertex(x + cos(a) * r2, y + sin(a) * r2);
    vertex(x + cos(a + half) * r1, y + sin(a + half) * r1);
  }
  endShape(CLOSE);
}

// 🎧 interacción
function mousePressed() {
  userStartAudio();
  if (!song.isPlaying()) song.loop();

  dragging = true;

  // 🌊 crear ripple
  ripples.push(new Ripple(mouseX, mouseY));
}

function mouseDragged() {
  // 🌊 trail de ripples
  ripples.push(new Ripple(mouseX, mouseY));
}

function mouseReleased() {
  dragging = false;
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  pg = createGraphics(width / scaleFactor, height / scaleFactor);
  pg.pixelDensity(1);
}
```
<img width="922" height="719" alt="Captura de pantalla 2026-04-16 223026" src="https://github.com/user-attachments/assets/41004c9b-b6e6-42cb-8bb5-9f9ea2b13396" />
<img width="919" height="716" alt="Captura de pantalla 2026-04-16 223105" src="https://github.com/user-attachments/assets/6de5b6e5-21e2-43c6-b8d6-c5b79bef9e96" />



[**LINK AL PROYECTO**](https://editor.p5js.org/elennc/full/80YnJOM67)

## Bitácora de reflexión
