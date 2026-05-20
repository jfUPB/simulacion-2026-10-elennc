# Unidad 8

## Bitácora de proceso de aprendizaje
### Actividad 01

1. *Indica qué herramienta te interesa explorar y por qué.*
    
    Quiero explorar blender ya que a lo largo de la carrera he visto las cosas que hacen ahí pero nunca he sido muy buena en eso, entonces quiero explorarlo.
    
2. *Explica qué relación tiene esa herramienta con tu línea de énfasis o interés profesional.*
    
    Mi línea de énfasis es animación entonces sí vemos más Blender que otras líneas, mas en lo que yo siempre me he enfocado ha sido el arte para pre producción, como character sheets, storyboard, diseño de personaje, etc., así que sería bueno tener más experiencia.
    
3. *Busca 2 o 3 referentes realizados con esa herramienta o cercanos a su ecosistema.*
    
<img width="321" height="287" alt="Captura de pantalla 2026-05-20 001422" src="https://github.com/user-attachments/assets/42596a11-502a-4ed5-b54c-6c0f27d2dcb7" />
<img width="222" height="238" alt="Captura de pantalla 2026-05-20 001405" src="https://github.com/user-attachments/assets/eef7304c-efce-458a-888c-0491557fe9f2" />

    
4. *Explica qué te interesa de esos referentes.*
    
    Me gusta que son muy brillantes y fáciles de entender como visualizadores de audio, no complican mucho la estética y hace que funcionen solo como “background”
    
5. *Propón uno o dos posibles contextos profesionales para tu pieza final.*
    
    Visuales para conciertos por ejemplo, o presentaciones en vivo de cualquier tipo. También podría funcionar como visualizador en una app de música.
    

### Actividad 02

1. *Indica qué sistema del curso vas a transferir.*
    
    Me gustaría empezar con sistemas de partículas y la aplicación de fuerzas a éstas.
    
2. *Explica brevemente cómo funcionaba ese sistema en p5.js.*
    
    En p5.js un sistema de partículas funcionaba con un emisor del que salían “cuerpos” con una vida limitada. El comportamiento de las partículas dependía de las fuerzas que le quisiéramos agregar, como gravedad, viento, fricción, etc., por medio del cálculo de la aceleración teniendo en cuenta Motion 101.
    
3. *Justifica por qué quieres transferirlo a la herramienta elegida.*
    
    Quiero transferirlo ya que es de las cosas que me quedó más clara de todas las unidades y es lo que me parece que se ve más interesante visualmente, ya que se ve muy fluido y natural pero al mismo tiempo se pueden tener muchos tipos de parámetros que hacen que se vea variado. Además, a pesar de no haber tenido demasiado contacto con Blender, sí he utilizado sistemas de partículas ahí entonces me siento un poco más cómoda intentándolo.
    
4. *Explica qué tipo de pieza visual te imaginas construir con esa combinación.*
    
    Quiero realizar un visual de una canción, tipo lo que pondrían en las pantallas de fondo de un concierto. Quiero seguir el concepto que he utilizado antes de cosas tipo “galaxia” o “sueños,” cosas que son muy mágicas.
    
5. *Señala qué dificultades técnicas anticipas.*
    
    Sé que va a ser un poco complicado llegar exactamente a lo que quiero yo sola ya que no sé utilizar bien el recurso, pero creo que ya entendiendo más o menos el funcionamiento de un sistema de partículas desde p5.js puedo llegar a algo bueno. Probablemente sea complicado renderizar, ya que el tener muchas partículas hace que se vea mejor pero es más pesado para el PC, entonces tendré que buscar una forma de optimizarlo.
    

### Actividad 03

1. *Describe qué componentes o módulos necesitas aprender en tu herramienta.*
    
    Principalmente necesito entender cómo funcionan los sistemas de partículas en Blender y cómo puedo aplicarles fuerzas de forma que puedan reaccionar al sonido.
    
2. *Realiza al menos dos pruebas técnicas.*

[Grabación de pantalla 2026-05-16 191712.mp4](attachment:2f9de882-27e1-4d4e-9d6e-039fbd87736b:Grabacin_de_pantalla_2026-05-16_191712.mp4)

1. *Explica qué resuelve cada prueba.*

Hice una prueba en la que sólo me fijaba cómo respondía un sistema de partículas a una canción utilizando un bakeo de sonido de blender, que básicamente transforma en números que dan la intensidad del movimiento en un keyframe.

La siguiente prueba que hice fue más de optimización, intentando bakear la animación en un objeto como alembic para poder reducir un poquito el peso para poder renderizar y trabajar más fácil.

1. *Indica qué parte del sistema ya lograste reconstruir.*
    
    Ya tengo un sistema de partículas que reacciona a la música y está siendo afectado por diferentes fuerzas.
    
2. *Explica qué parte sigue sin resolverse.*
    
    Ahora mismo necesito ajustar la sensitividad al sonido ya que no es tan fuerte como me gustaría, además de intentar darle más movimiento para que sea más interesante.
    

### Actividad 04

*Construye una tabla, esquema o mapa comparando:*

- *cómo funcionaba el sistema en p5.js,*
- *cómo se implementa en la nueva herramienta,*
- *qué se mantiene,*
- *qué cambia,*
- *qué ventajas aparecen,*
- *qué limitaciones nuevas surgen.*

*Cierra respondiendo:*

*¿Qué aprendiste sobre el sistema al tener que reconstruirlo fuera de p5.js?*

| SISTEMA | p5.js | Blender |
| --- | --- | --- |
| Sistema de partículas | Se crean a través de métodos y arrays desde un emisor. | Se crean desde un emisor con la herramienta ya existente de creación de sistemas de partículas |
| Movimiento | Se utiliza Motion 101 calculando cosas como posición, velocidad y aceleración, y a esto se le agregan fuerzas que alteran el flujo que debo crear por medio de código desde cero. | Hay una simulación “base” por así decirlo de un sistema de partículas, pero se puede controlar todo desde sliders y otros parámetros sin tener que manualmente calcular aceleración, velocidad y posición. Las fuerzas ya están rpedeterminadas en Blender y solo necesito agregarlas. |
| Sonido | Utiliza el audio para convertir amplitud o frecuencias en diferentes parámetros que modifican el movimiento de las partículas | De igual forma utiliza las propiedades del audio para crear parámetros que alteran la geometría del objeto. |

El mayor cambio es que en p5.js hay que hacer todo manualmente desde código, mientras que en Blender muchas cosas ya están predeterminadas y listas para ser usadas lo que hace el proceso más rápido y tal vez más fácil de entender, como un emisor de partículas o las fuerzas mismas, además de los parámetros individuales que determinan cada comportamiento de estas. Igualmente creo que el hecho de que Blender tenga tantas variables y posibilidades hace que sea un poquito abrumador intentar aprender, mientras que en p5.js se siente como un aprendizaje más desde cero de a poquitos.

El pasar el sistema a Blender me ayudó a visualizar de mejor forma la relación entre las fuerzas y las partículas ya que no tenía que preocuparme mucho por si el código estaba funcionando y todo eso, sino que con leer el nombre de los parámetros preestablecidos lograba entender qué podía modificar de cada uno.


## Bitácora de aplicación 
### Actividad 05

1. *Herramienta elegida.*
    
    Blender
    
2. *Sistema transferido.*
    
    Sistema de partículas y reacción a fuerzas
    
3. *Contexto profesional concreto.*
    
    Visual tipo concierto
    
4. *Concepto visual.*
    
    Magia, estrellas, mirella, muy y2k.
    
5. *Referencias.*

<img width="683" height="565" alt="Captura de pantalla 2026-05-19 235653" src="https://github.com/user-attachments/assets/d332a41d-4576-4c0a-a5c4-5da0292db4f7" />


1. *Bocetos.*
    
    <img width="1264" height="531" alt="Captura de pantalla 2026-05-20 000016" src="https://github.com/user-attachments/assets/c8ae77c0-3034-4b07-b82f-3b27bd3a6062" />

    
2. *Explicación de la transferencia.*
    
    Antes en p5.js teníamos que construir el sistema de partículas desde cero, agregar las fuerzas desde cero y calcular el movimiento desde cero, pero todo esto son herramientas que ya están dentro de blender, entonces se vuelve un poco más rápido y fluído el proceso, además de que es un poco más visual con los nodos que con código, pero es como si utilizáramos funciones ya prehechas de p5.js.
    
3. *Mapa de decisiones.*
4. *Mapa de presentación.*
    
    <img width="976" height="529" alt="Captura de pantalla 2026-05-20 000457" src="https://github.com/user-attachments/assets/85c1aaf4-ba50-4275-b1ce-b215942b0bc9" />
    
5. *Evidencia del uso de IA.*
    
    Uitlicé IA para guiarme cuando un nodo no funcionaba o cuando necesitaba una alternativa a algo, pero en su mayoría me guié de videos de youtube sobre blender y sistemas de partículas.
    
6. *Código, archivo, proyecto o documentación técnica según la herramienta.*
    
    [LINK DE DRIVE](https://drive.google.com/drive/folders/1kjXmO60pCN_4xcd9P830lBoT7KiMd9Ki?usp=drive_link)
    
7. *Registro visual de la pieza.*
    
    [LINK DE YOUTUBE](https://youtu.be/rozLYRUmplY)

   

https://github.com/user-attachments/assets/f20d43bc-a4f4-43ae-8d69-c5bb56c9c914



## Bitácora de reflexión
