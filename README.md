# Tarea 3 · La factura bajo el microscopio
Pareja: Manuel Bono Torres y Gonzalo · GitHub: Zombneitor123, gonzaloaragar-svg

## Reto 1 · Compilar desde la terminal
- Comando usado y resultado (captura: capturas/reto1.png)
  - `javac -version`: Comprobamos la versión del compilador del JDK (`javac 21.0.12.1`).
  - `javac Factura.java`: Intentamos compilar el código fuente. El compilador devuelve 2 errores y la compilación se detiene:
    - `Factura.java:15: error: illegal character: '#'`
    - `Factura.java:17: error: ';' expected`
  - `dir Factura.class`: Comprobamos que no se ha generado ningún archivo `.class`.
  - Captura añadida en: `capturas/reto1.png`.
- ¿Qué es Factura.java? ¿Y Factura.class? ¿Quién ejecuta el .class?
  - **`Factura.java`**: Es el archivo de **código fuente**. Contiene las instrucciones del programa escritas en lenguaje Java, comprensibles y editables por el desarrollador.
  - **`Factura.class`**: Es el archivo que contendrá el **bytecode** resultante de compilar el código fuente. Es un formato binario intermedio, independiente de la arquitectura física.
  - **¿Quién ejecuta el `.class`?**: Lo ejecuta la **JVM (Java Virtual Machine / Máquina Virtual de Java)** mediante el comando `java Factura`. La JVM traduce el bytecode a código máquina nativo del sistema operativo y procesador que lo ejecuta.
- ¿Por qué no se ha generado Factura.class la primera vez?
  - Porque el compilador `javac` detiene el proceso al encontrar errores léxicos y sintácticos en el código fuente. El compilador de Java solo produce el archivo de bytecode (`.class`) si el código fuente compila limpiamente al 100%. De esta forma evita generar ejecutables incompletos o corruptos.

## Reto 2 · Errores de compilación
| Nº | Línea | Mensaje de javac | Tipo (léxico / sintaxis / semántica) | Corrección |
|----|-------|------------------|--------------------------------------|------------|
| 1  |       |                  |                                      |            |
| 2  |       |                  |                                      |            |
| 3  |       |                  |                                      |            |
Captura del programa funcionando: capturas/reto2.png

## Reto 3 · Depuración del error lógico
| Variable | Valor esperado | Valor real |
|----------|----------------|------------|
| descuento |               |            |
| total     |               |            |
- Línea del error y explicación:
- Corrección:
- Mi propia prueba (producto, precio, cantidad, socio → total):
Captura del depurador: capturas/reto3.png

## Reto 4 · Reutilización
- Métodos creados y para qué sirve cada uno:
- ¿Qué ventaja tiene frente al código anterior?

## Reto 5 · ¿Qué lenguaje elegirías?
- Caso A (web de la tienda):
- Caso B (propuesta en ensamblador):
