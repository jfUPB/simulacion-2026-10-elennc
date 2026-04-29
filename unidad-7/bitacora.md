# Unidad 7

## Bitácora de proceso de aprendizaje
### Actividad 01

Una palabra de Lee que me interesa es Moon, ya que utiliza las dos O’s como si fueran la Tierra y la Luna haciendo que una orbite alrededor de la otra

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/e7905fc2-c5a6-4b6a-bece-4093902b2298" />


Otra fue la de shark, que me costó un poquito entenderla porque la mayoría de ejemplos solo utilizan como una de las letras para mostrar el significado, pero este usa todas las letras para mostrar el tiburón completo pero de una forma abstracta.

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/ec10ec84-5810-4059-8917-4d8ce1c09b58" />


Por último me gustó mucho la de Tunnel ya que se entiende incluso antes de que la palabra entera esté formada, al hacer que todas las letras expresen el significado de la palabra.

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/9b2fbc8f-4d00-450e-8ca4-d787ff477020" />


Tres palabras que podría hacer:

- “Thorns” y que físicamente le salgan pinchos a la palabra como si fuera un arbusto
- “Freefall” y que sean las letras cayendo del cielo, caída libre
- “Skipping” y que sean piedras lanzadas en el agua saltando. Ésta es la idea que más me interesa, creo que podría ser muy divertido si logro plasmar la visión de mi cabeza.

### Actividad 02

1. *Explica con tus palabras qué hace cada uno de esos conceptos.*
    - `Engine` : contiene métodos para manipular la simulación del mundo
    - `World` : es un conjunto complejo de una o más partes. ahora se llama composite.
    - `Bodies` :  cuerpos rígidos a los que afectan fuerzas
    - `Constraint` : maneja restricciones entre cuerpos como la distancia
    - `MouseConstraint` : restricciones que permiten interacción del usuario como la capacidad de mover cuerpos.
2. *Replica al menos dos experimentos básicos integrando Matter.js con p5.js. Incluye código y capturas o enlaces.*
    
    [BOLAS REBOTANDO](https://editor.p5js.org/elennc/full/JABdvLK6N)
    
    [CAJAS CAYENDO](https://editor.p5js.org/elennc/full/Bd7LPltqK)
    
3. *Describe qué tipo de comportamiento físico te interesa explorar en tu palabra.*
    
    Para hacer el de skipping me gustaría que las letras salgan de la izquerda y se deslicen hasta la derecha pero saltando como lo hacen las piedras en el agua, además dejando ondas donde tocan como si hubiese agua realmente. 
    

### Actividad 03

1. *Realiza al menos dos experimentos simples de audio-reactividad.*
2. *Explica qué dato estás leyendo del audio.*
3. *Explica qué comportamiento visual o físico activa ese dato.*
4. *Describe qué tipo de respuesta sonora te serviría más para tu palabra y por qué.*

[MOVER CÍRCULO CON AUDIO](https://editor.p5js.org/elennc/full/X9jxsKC4x)

Este ejemplo lee la frecuencia del sonido, mapeando la posición del círculo en el eje Y para sonidos agudos y en el eje X para sonidos graves. Si el sonido es agudo el círculo sube, y si es grave va hacia la derecha.

[CÍRCULO QUE CAMBIA DE TAMAÑO](https://editor.p5js.org/elennc/full/vuPVjac5z)

Este círculo cambia de tamaño dependiendo del volumen del sonido.

Creo que la forma de integrar la reacción a mi palabra sería hacer que los saltos de las letras dependan del sonido, y tener un track con más o menos tres sonidos de saltos para que todas puedan utilizarlo de guía.

### Actividad 04

[PRUEBA: FREEFALL](https://editor.p5js.org/elennc/full/F2IzWtsuu)

Utilicé el audio de viento para generar un temblor en las letras individualmente dependiendo de la amplitud o volumen del sonido, para que parezca que están cayendo. Esto simula como el aire que no es uniforme afecta a un objeto que cae, haciendo que no sea totalmente uniforme el movimiento.

## Bitácora de aplicación 
### Actividad 05

Fuente redondeada: más juguetón, menos serio

Sonido: por cada letra se reduce 1/4 de su volumen cada vez que la letra toca el agua, y la intensidad del sonido determina qué tan alto salta la próxima vez

Ripples: efecto de agua para tener más realismo

Drag: se pueden arrastrar las letras y dejarlas caer para que sean objetos sobre el agua. 

Como quise que cada letra fuera una “piedra”, a cada una la afecta la gravedad y tiene una sensación de peso, y con esta interacción podía convertir solo letras en algo que se interpreta más como un objeto de la vida real.

<img width="831" height="563" alt="Captura de pantalla 2026-04-28 232944" src="https://github.com/user-attachments/assets/1dfd01bb-7ee2-474a-af5a-48e48c638974" />

<img width="1128" height="195" alt="image" src="https://github.com/user-attachments/assets/b0e0f275-2496-49ea-9c6e-462c73751d76" />

<img width="2360" height="1640" alt="Untitled_Artwork" src="https://github.com/user-attachments/assets/e1e6c065-6918-45b5-a208-1fdd3e2b3f18" />


[LINK AL PROYECTO](https://editor.p5js.org/elennc/full/DRi-FcG2_)

## Bitácora de reflexión
