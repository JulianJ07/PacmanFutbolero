# Pacman Futbolero en Jack

## Hecho por

- Julian Jimenez
- Alejandro Cifuentes
---
Juego tipo Pac-Man hecho en Jack para Nand2Tetris. El jugador controla un futbolista dentro de un laberinto, recoge conos de entrenamiento, trofeos y balones de poder mientras evita a los futbolistas rivales.

## Como ponerlo a funcionar

1. Abre el VM Emulator:
   `C:\Users\Julian\Downloads\nand2tetris\nand2tetris\tools\VMEmulator.bat`

2. En el VM Emulator selecciona:
   `File > Load Program`

3. Carga la carpeta completa:
   `C:\Users\Julian\Documents\Codex\2026-04-28\pacman futbolero`

4. En la parte superior cambia `Animate` a:
   `No animation`

5. Sube la velocidad hacia `Fast`.

6. Presiona el boton de ejecutar continuo, el de doble flecha `>>`.

Si la pantalla queda quieta, presiona `Stop`, luego `Reset`, y vuelve a ejecutar con `>>`.

## Controles

- Flecha izquierda: mover a la izquierda.
- Flecha arriba: mover hacia arriba.
- Flecha derecha: mover a la derecha.
- Flecha abajo: mover hacia abajo.
- R: reiniciar la partida.
- ESC: salir del juego.

## Objetivo del juego

El objetivo es recorrer el laberinto y recoger todos los objetos sin ser atrapado por los futbolistas rivales.

- Conos de entrenamiento: dan 10 puntos.
- Trofeos: dan 50 puntos.
- Balones: dan 100 puntos y activan un poder temporal.
- Si tienes el poder del balon activo, el jugador se ve mas grande, titila y los rivales intentan alejarse de ti. Puedes tocar a un rival para derrotarlo. El rival reaparece en el centro, queda en reposo unos 5 segundos y luego vuelve a perseguirte.
- Si un rival te toca sin tener poder activo, termina la partida.
- Ganas cuando recoges todos los objetos del tablero.
- Los rivales se liberan por progreso: el primero sale al iniciar, el segundo al 25%, el tercero al 50%, el cuarto al 75% y el quinto al 100%.

## Como funciona el codigo

`Main.jack` es el punto de entrada. Crea el objeto `Game`, ejecuta el juego y luego libera memoria.

`Game.jack` controla el ciclo principal del juego. Lee el teclado con `Keyboard.keyPressed()`, actualiza el puntaje, activa el poder del balon, libera rivales segun el porcentaje del mapa completado, revisa colisiones y determina si el jugador gana, pierde o reinicia con la tecla `R`. Las colisiones ignoran temporalmente a los rivales que estan en reposo despues de ser derrotados.

`Board.jack` guarda la estructura del laberinto. Tambien dibuja el mapa, los conos, los trofeos y los balones usando `Screen.drawRectangle`. El fondo del tablero se dibuja en negro y las paredes son bloques blancos para que el laberinto se vea mas claro en blanco y negro.

`Player.jack` representa al futbolista principal. Tiene su propia posicion, metodo de movimiento y metodo de dibujo. Cuando el poder del balon esta activo, cambia a un sprite mas grande y alterna entre dos dibujos para simular titileo.

`Rival.jack` representa a cada futbolista rival. Cada rival puede estar activo o esperando dentro de la jaula. Cuando `Game.jack` lo libera, abandona la jaula por la puerta central y despues busca al jugador calculando el camino mas corto por las celdas transitables del laberinto, evitando quedarse chocando contra paredes. Si el jugador tiene el poder del balon, usa el mismo mapa de distancias para elegir una celda mas lejana y alejarse. Cuando el jugador toca a un rival con el poder activo, el rival reaparece en el centro, se dibuja en modo reposo durante unos 5 segundos y despues vuelve a salir para perseguir al jugador.

## Algoritmo de persecucion de los rivales

El tablero se maneja como una cuadricula de 21 filas por 31 columnas. Cada casilla se convierte en un numero con la formula `fila * 31 + columna`, lo que permite guardar informacion del camino en arreglos simples de Jack.

Para perseguir al jugador, `Game.jack` llama a `Rival.beginPathFrame()` antes de mover los rivales. Cuando el primer rival necesita moverse, `Rival.jack` construye un mapa de distancias desde la posicion actual del jugador usando una busqueda por amplitud, tambien conocida como BFS. Esta busqueda empieza en la celda del jugador, revisa las celdas vecinas que no son paredes con `board.canEnter()`, y guarda en `pathDistance` cuantos pasos cuesta llegar a cada celda transitable.

Despues de construir ese mapa, cada rival solo mira sus cuatro vecinos: derecha, abajo, izquierda y arriba. Si el jugador no tiene poder activo, el rival escoge el vecino con la distancia mas pequena. Eso significa que siempre avanza por el camino mas corto disponible y no intenta atravesar paredes.

Cuando el jugador recoge un balon y el poder esta activo, se usa el mismo mapa de distancias pero con la decision al reves: el rival escoge el vecino con la distancia mas grande. Asi intenta alejarse del jugador respetando los pasillos del laberinto.

Para reducir el lag, el mapa de distancias no se recalcula por cada rival. Se calcula una sola vez por ciclo de movimiento y todos los rivales lo reutilizan. Ademas, el arreglo `pathDistance` no se limpia completo en cada ciclo; solo se limpian las celdas visitadas en la busqueda anterior. Esto es importante en la VM de Nand2Tetris porque el rendimiento es limitado y al avanzar la partida hay mas rivales activos.

La velocidad de los rivales tambien se controla dentro de `Rival.jack`: alternan entre esperar 2 y 3 ciclos antes de moverse. Esto deja la persecucion un poco mas lenta que antes, aproximadamente al 80% de la velocidad previa, sin hacer mas lento el movimiento del jugador.

## Compilacion

El proyecto ya incluye archivos `.vm`. Si modificas los `.jack`, vuelve a compilar la carpeta con este ejemplo:

`C:\Users\Julian\Downloads\nand2tetris\nand2tetris\tools\JackCompiler.bat "C:\Users\Julian\Documents\Codex\2026-04-28\pacman futbolero"`
