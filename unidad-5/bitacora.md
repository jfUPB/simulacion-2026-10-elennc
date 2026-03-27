# Unidad 5
## Bitácora de proceso de aprendizaje
### Actividad 01

**Capa de comportamiento:**

1. *¿Qué propiedades tiene cada partícula? Clasifícalas: ¿Cuáles definen su estado físico? ¿Cuáles su estado vital?*
    
    Son partículas redondas que nacen en un punto específico y caen en la pantalla, perdiendo opacidad en su recorrido hasta desaparecer. Igualmente utiliza motion101
    
2. *¿Qué condición determina que una partícula “muere”? ¿Es una muerte instantánea o gradual?*
    
    El atributo de lifespan es el que determina qué tanto tiempo vive la partícula. Cuando el lifespan es menor o igual a 0, la partícula se considera muerta. Es una muerte gradual ya que el lifespan se inicia en 255, pero empieza a disminuir 2 en cada frame hasta que llega a 0. Este valor se usa al dibujar el círculo que es la visualización de la partícula como la variable de la opacidad.
    
3. *¿Cómo se actualiza la partícula en cada frame? Identifica el patrón motion 101 dentro de la partícula.*
    
    En cada frame a la velocidad se le suma la aceleración, y dicha velocidad resultante se le suma a la posición. Después se reduce el lifespan y se resetea la aceleración para prepararse al siguiente frame.
    
    ```jsx
    update() {
        this.velocity.add(this.acceleration);
        this.position.add(this.velocity);
        this.lifespan -= 2;
        this.acceleration.mult(0);
      }
    ```
    

**Capa de estructura:**

1. *¿Quién crea las partículas? ¿En qué momento?*
    
    Las partículas se crean en sketch.js utilizando la función de push( ) para llenar un array llamado particles[ ]
    
2. *¿Quién decide cuándo eliminar una partícula del array?*
    
    Hay un ciclo que recorre el array de atrás para adelante checkeando si el método isDead está retornando un valor menor a 0, y si este es el caso, se borra la partícula.
    
3. *¿Por qué se recorre el array en orden inverso para eliminar? ¿Qué pasaría si no se hiciera así?*
    
    Se recorre al revés para evitar saltarse puestos del array, pues cuando elimina una partícula todas las que siguen después se mueven un puesto hacia atrás mientras que el contador sigue avanzando, haciendo que la partícula que ya ocupa el puesto de la eliminada no sea checkeada. Si corremos el array al revés, las partículas se mueven hacia atrás al eliminar una pero la que me estoy saltando ya fue checkeada antes.
    
4. *Si no eliminaras nunca las partículas, ¿Qué pasaría con la memoria y el rendimiento? Haz el experimento: comenta la línea que elimina y observa el frame rate.*
    
    El equipo no podría sostener infinitas partículas, y estaría gastando cantidades impresionantes de memoria solo calculando posiciones y nuevos datos para cada frame cuando ya no es necesario porque no las estamos visualizando.
    

**Capa de visualización:**

1. *¿Qué elementos visuales usa para representar una partícula?*
    
    En este caso estamos utilizando círculos y con su opacidad podemos ver qué tan cerca o no está de morir.
    
2. *¿Cómo se conecta el “tiempo de vida” con la apariencia visual?*
    
    El atributo de lifespan no solo se usa para literalmente matar la partícula sino que se vuelve a usar en el momento de dibujar el círculo en el lugar donde iría el valor de la opacidad. Esto hace que entre mayor sea el lifespan (máximo al acabar de nacer) la partícula sea más opaca y se vuelva más transparente con el transcurso del tiempo hasta desaparecer.
    
3. *Si quisieras cambiar la representación visual (por ejemplo, usar líneas en vez de círculos), ¿Qué cambiarías y qué NO cambiarías?*
    
    Para cambiar el círculo lo único que cambia es el método show( ) de la clase. Tendría que cambiar el círculo por una línea tomando en cuenta que la función para dibujar una línea acepta 4 parámetros.
    
    ```jsx
    show() {
        stroke(0, this.lifespan);
        strokeWeight(2);
        fill(127, this.lifespan);
        //circle(this.position.x, this.position.y, 8);
        line(this.position.x, this.position.y, this.position.x, this.position.y-10)
      }
    
    ```
    

### Actividad 02

1. *¿Qué responsabilidades que antes estaban en `draw()` ahora están dentro de la clase `Emitter`?*
    
    La clase Emitter tiene un método llamado run( ) que toma la función de recorrer el array de partículas. En el ejemplo anterior esto pasaba dentro de draw( )
    
2. *¿Cuál es la ventaja de encapsular la lógica de emisión en una clase separada?*
    
    Hace que se pueda referir a ella la cantidad de veces necesaria sin tener que estar reescribiendo el pedazo de código.
    
3. *En este ejemplo hay un array de emitters. ¿Quién crea los emitters? ¿Quién crea las partículas dentro de cada emitter?*
    
    Los emitters que llenan el array se crean durante la función mousePressed( ) en sketch.js. Dentro de cada emitter se crean las partículas en la función draw( ) utilizando el método addParticle( ) de la clase emitter.
    
4. *Dibuja un diagrama que muestre la jerarquía: `sketch → [emitters] → [partículas]`. ¿Cuántos niveles de “colección” hay?*
    
    

***Transferencia conceptual:***

1. *Describe este ejemplo usando palabras que NO mencionen p5.js, JavaScript, ni ninguna herramienta específica. Usa solo términos como: entidad, estado, colección, emisor, ciclo de vida, fuerza.*
    
    Cuando se da click en la pantalla, se llama una función que crea un objeto de tipo Emitter en un array de objetos. Al ser creado, en cada frame el objeto Emitter crea partículas dentro de un array propio, recorriendo este constántemente para verificar la vida de cada partícula.
    

### Actividad 03

1. *¿Qué tienen en común las subclases de partículas? ¿Qué tienen de diferente?*
    
    En este ejemplo tenemos la clase de partículas normales redondas, y una subclase llamada confetti que funciona exactamente igual ya que hereda todos los métodos y atributos de la clase padre (particula). Lo único que cambia es que en Confetti la figura que se dibuja no es un círculo sino un cuadrado.
    
2. *¿Por qué es importante que el Emitter no necesite saber qué tipo específico de partícula está gestionando? Explica esto con tus propias palabras.*
    
    Porque ambos tipos de partícula funcionan igual, así que no hay necesidad de saber la diferencia después de pasar el condicional que decide qué partícula será creada en primer lugar.
    
3. *Si mañana quisieras agregar un tercer tipo de partícula, ¿Qué tendrías que crear y qué NO tendrías que modificar?*
    
    Puedo crear otro hijo de partícula, sin modificar el código original ni el código de confetti, y cambiar simplemente la forma en la que se dibujan las partículas nuevas utilizando una función show que sobreescriba la del padre. Si quiero cambiar algún otro método o atributo específico solo necesito sobreescribirlo en un nuevo código para el hijo.
    
4. *Compara con Example 4.2: ¿Cambió la lógica del Emitter? ¿Cambió la lógica de muerte? ¿Qué capa del sistema se modificó y cuáles permanecieron intactas?*
    
    No cambió la lógica, pues sigue utilizando el atributo de lifespan para verificar si una partícula está muerta o no, recorriendo un array desde atrás para evitar saltarse partículas. Lo único diferente es que en 4.2 no teníamos múltiples emisores, sino que las partículas nacían en un solo punto especificado de la pantalla, entonces no se está necesitando una clase específica para referirse al punto del que nacen las partículas ni nada de eso. 
    

### Actividad 04

**Fuerzas globales vs. locales:**

1. En Example 4.6, ¿Dónde se define la gravedad? ¿Quién la aplica a las partículas? ¿Es una fuerza global o local?
    
    Se define dentro de la función draw, y se aplica utilizando el método addForce( ) de la clase Emitter que agrega la gravedad a la aceleración marco motion101 a todas las partículas emitidas con el método addForce( ) de la clase Particle.
    
2. En Example 4.7, ¿Qué diferencia hay entre la gravedad y la fuerza del repeller? ¿Dónde “vive” cada una?
    
    La gravedad es una fuerza universal que afecta a todas las partículas haciendo que tiendan a caer, mientras que la fuerza repeller vive en la parte más baja de la pantalla y tiende a hacer que las partículas que la gravedad le tira intenten evitar el círculo que dibuja.
    
3. La fuerza del repeller depende de la distancia entre la partícula y el repeller. ¿Qué principio físico se está modelando?
    
    La fuerza de repulsión, puesto que la distancia y la fuerza son inversamente proporcionales: entre mayor la distancia del repeller a la partícula, menor será la fuerza del repeller.
    
4. ¿Cambió la clase Particle entre Example 4.6 y 4.7? ¿Qué implica esto sobre la separación entre comportamiento de la partícula y fuerzas externas?
    
    En 4.6 las partículas tienen una masa, y además como en 4.7 existe un segundo tipo de objeto que genera una fuerza, la forma en la que se aplican las fuerzas cambia para permitir agregar más que solo la gravedad a la hora de aplicar la primera ley de Newton.
    

**Tabla comparativa:**

Completa la siguiente tabla en tu bitácora:

| **Aspecto** | **4.2** | **4.4** | **4.5** | **4.6** | **4.7** |
| --- | --- | --- | --- | --- | --- |
| ¿Quién crea partículas? | draw( ) | Emitter | Emitter | Emitter | Emitter |
| ¿Hay clase Emitter? | No | Sí | Sí | Sí | Sí |
| ¿Hay herencia? | No | No | Sí | No | No |
| ¿Hay fuerzas externas? | Sí (gravedad) | Sí (gravedad) | Sí (gravedad) | Sí (gravedad) | Sí (gravedad, repeller) |
| ¿Hay interacción entre elementos? | No | No | No | No | Sí |
| ¿Cómo mueren las partículas? | draw( ) y particle.lifespan | emitter.run( ) y particle.lifespan | emitter.run( ), particle.lifespan, confetti.lifespan | emitter.run( ) y particle.lifespan | emitter.run( ) y particle.lifespan |

**Modificación quirúrgica:**

Elige UNA de estas modificaciones sobre el Example 4.7 e impleméntala:

- **(a)** Cambiar la visualización sin cambiar fuerzas ni estructura.
- **(b)** Cambiar las fuerzas sin cambiar la estructura ni la visualización.
- **(c)** Cambiar la condición de muerte sin cambiar la visualización ni las fuerzas.

![u5a4](https://github.com/user-attachments/assets/07e45e8b-beca-44de-b828-c35e46f3e024)

Para la modificación que elegiste, responde:

- ¿Qué líneas de código tocaste?
    
    Cambié las líneas que aluden a figuras y colores en repeller y en partículas.
    
- ¿Qué clases/funciones modificaste?
    
    Cambié el método show( ) del repeller, el método show de las partículas y el color de fondo.
    
- ¿Qué partes del programa NO necesitaste modificar?
    
    No modifiqué nada del funcionamiento de las partículas o repeller, solo la forma en la que se representan en la pantalla.
    
- ¿Por qué fue posible hacer este cambio sin afectar las demás capas?
    
    Porque cambié solo elementos que están dentro de sus respectivas clases evitando que mis cambios afecten a todo el proyecto. Además utilicé push( ) y pull( ) para crear grupos y hacer cambios en partes específicas del código.

## Bitácora de aplicación 
### Actividad 05

**Concepto:** Quise representar el ciclo de vida de las hojas de los árboles. Las hojas nacen verdes, y lentamente se transforman a cafés, y al caer al piso se descomponen y desaparecen.

**Boceto:** 

<img width="1368" height="270" alt="Captura de pantalla 2026-03-27 100905" src="https://github.com/user-attachments/assets/031d13bb-58a6-422b-aede-4674a477f58a" />
<img width="487" height="392" alt="Captura de pantalla 2026-03-27 101023" src="https://github.com/user-attachments/assets/91c8ec65-495f-4ab5-bbbd-f65c2dd82e84" />


```jsx
let leaves = [];
let windForce = 0;
let windTimer = 0;

function setup() {
  createCanvas(600, 600);
}

function draw() {
  background(200, 230, 255);
  noStroke()
  fill('#73A23C')
  rect(0,400,600, 250)

  drawTree();

  // viento temporal
  if (windTimer > 0) {
    windForce = 0.2;
    windTimer--;
  } else {
    windForce = 0;
  }

  // generar hojas constantemente
  if (random() < 0.05 + windForce) {
    leaves.push(new Leaf(random(width/2 - 50, width/2 + 50), 150));
  }

  // actualizar hojas
  for (let i = leaves.length - 1; i >= 0; i--) {
  let leaf = leaves[i];

    leaf.applyForce(createVector(windForce, 0.1)); // viento + gravedad
    leaf.update();
    leaf.display();

    // transición a "muerte"
    if (leaf.isDead()) {
      leaves.splice(i, 1);
    }

    // si toca el suelo, se transforma
    if (leaf.pos.y > height - 120 && leaf instanceof Leaf && !(leaf instanceof FadingLeaf)) {
      leaves[i] = new FadingLeaf(leaf.pos.x, leaf.pos.y, leaf.colorProgress);
    }
  }
}

// 🌳 árbol 
function drawTree() {
  
  canvas=createGraphics(width,height)
  
  push()
    canvas.translate(300,300)
    canvas.noStroke()
    canvas.fill(189, 169, 115);
    canvas.rectMode(CENTER)
    canvas.rect(0, 0, 90, 300);
    //canvas.fill('red')
    canvas.ellipse(-45,150,70)
    canvas.ellipse(45,150,70)
    canvas.erase()
    //canvas.rectMode(CENTER)
    canvas.rect(0,200,600,100)
    canvas.noErase()
  
    
  
    canvas.fill(50, 150, 50);
    canvas.ellipse(0, -150, 200, 200);
    canvas.ellipse(-80, -50, 200, 200);
    canvas.ellipse(80, -50, 200, 200);
  pop()
  
  image(canvas,0,0)
}

// 🖱 interacción
function mousePressed() {
  windTimer = 30; // dura ~1 segundo
}

// 🍃 clase base
class Leaf {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, random(1, 2));
    this.acc = createVector(0, 0);

    // ciclo de vida
    this.age = 0;
    this.maxAge = random(200, 300);

    // color (0 = verde, 1 = café)
    this.colorProgress = 0;

    // 🌬️ zigzag (Perlin noise)
    this.xOffset = random(1000);
    this.swayAmount = random(0.5, 2);
    this.noiseSpeed = random(0.01, 0.03);

    // 🍂 rotación
    this.angle = random(TWO_PI);
    this.rotationSpeed = random(-0.05, 0.05);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    // física básica
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);

    // 🌿 zigzag suave
    let sway = map(noise(this.xOffset), 0, 1, -1, 1);
    this.pos.x += sway * this.swayAmount;
    this.xOffset += this.noiseSpeed;

    // 🍂 rotación
    this.angle += this.rotationSpeed;

    // ⏳ envejecimiento
    this.age++;

    // 🎨 cambio de color progresivo
    this.colorProgress = this.age / this.maxAge;
    this.colorProgress = constrain(this.colorProgress, 0, 1);
  }

  display() {
    // transición verde → amarillo → café
    let green = color('#8BC34A');
    let yellow = color(220, 180, 50);
    let brown = color(150, 80, 20);

    let mid = lerpColor(green, yellow, this.colorProgress);
    let finalColor = lerpColor(mid, brown, this.colorProgress);

    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.angle);

    noStroke();
    fill(finalColor);
    ellipse(0, 0, 24, 12);

    pop();
  }

  // 💀 clave para el sistema (Nature of Code style)
  isDead() {
    return this.age > this.maxAge;
  }
}

// 🍂 hoja que se descompone (hereda de Leaf)
class FadingLeaf extends Leaf {
  constructor(x, y, progress) {
    super(x, y);
    this.lifespan = 255;
    this.colorProgress = progress;
    this.vel = createVector(0, 0); // ya no se mueve
  }

  update() {
    this.lifespan -= 3;
  }

  display() {
    let brown = color(150, 80, 20, this.lifespan);
    noStroke()
    fill(brown);
    ellipse(this.pos.x, this.pos.y, 24, 12);
  }

  isDead() {
    return this.lifespan <= 0;
  }
}
```
<img width="746" height="730" alt="Captura de pantalla 2026-03-27 101544" src="https://github.com/user-attachments/assets/7686a23a-1741-4ad3-b648-d992e2f92e4c" />
<img width="743" height="636" alt="Captura de pantalla 2026-03-27 101647" src="https://github.com/user-attachments/assets/e284a619-961b-484f-986c-d4245a70111c" />
<img width="741" height="732" alt="Captura de pantalla 2026-03-27 101626" src="https://github.com/user-attachments/assets/b28f9fad-81fe-4ffb-bf6c-7404c5b9eb7c" />



[LINK DEL PROYECTO](https://editor.p5js.org/elennc/full/1E5gVf56Y)



## Bitácora de reflexión
