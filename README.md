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
- Si tienes el poder del balon activo, el jugador se ve mas grande, titila y puedes tocar a un rival para derrotarlo y mandarlo de vuelta al centro.
- Si un rival te toca sin tener poder activo, termina la partida.
- Ganas cuando recoges todos los objetos del tablero.
- Los rivales se liberan por progreso: el primero sale al iniciar, el segundo al 25%, el tercero al 50%, el cuarto al 75% y el quinto al 100%.

## Como funciona el codigo

`Main.jack` es el punto de entrada. Crea el objeto `Game`, ejecuta el juego y luego libera memoria.

`Game.jack` controla el ciclo principal del juego. Lee el teclado con `Keyboard.keyPressed()`, actualiza el puntaje, activa el poder del balon, libera rivales segun el porcentaje del mapa completado, revisa colisiones y determina si el jugador gana, pierde o reinicia con la tecla `R`.

`Board.jack` guarda la estructura del laberinto. Tambien dibuja el mapa, los conos, los trofeos y los balones usando `Screen.drawRectangle`. El fondo del tablero se dibuja en negro y las paredes son bloques blancos para que el laberinto se vea mas claro en blanco y negro.

`Player.jack` representa al futbolista principal. Tiene su propia posicion, metodo de movimiento y metodo de dibujo. Cuando el poder del balon esta activo, cambia a un sprite mas grande y alterna entre dos dibujos para simular titileo.

`Rival.jack` representa a cada futbolista rival. Cada rival recuerda su posicion inicial y puede estar activo o esperando dentro de la jaula. Cuando `Game.jack` lo libera, abandona la jaula por la puerta central y despues se mueve de forma mas lineal: conserva direccion por varios pasos, recalcula hacia el jugador cada cierto tiempo y gira cuando encuentra una pared. Cuando el jugador tiene el poder del balon y toca a un rival, el rival vuelve a su posicion inicial.

## Nota sobre colores

La pantalla oficial de Nand2Tetris no soporta colores reales como rojo, azul o amarillo. La API `Screen` solo permite dibujar pixeles negros o blancos. Por eso los futbolistas rivales no pueden verse rojos de verdad dentro del VM Emulator oficial; en su lugar tienen un uniforme rayado para distinguirlos del jugador.

## Compilacion

El proyecto ya incluye archivos `.vm`. Si modificas los `.jack`, vuelve a compilar la carpeta con:

`C:\Users\Julian\Downloads\nand2tetris\nand2tetris\tools\JackCompiler.bat "C:\Users\Julian\Documents\Codex\2026-04-28\pacman futbolero"`
