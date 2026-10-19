# Tarea 3 · La factura bajo el microscopio
Pareja: Manuel Bono Torres y Gonzalo Aragón García · GitHub: Zombneitor123, gonzaloaragar-svg

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
| 1  | 15    | `error: illegal character: '#'` | Léxico | Eliminar el carácter `#` del identificador: `double precio = sc.nextDouble();` |
| 2  | 17    | `error: ';' expected` | Sintaxis | Añadir el punto y coma `;` al final de la sentencia: `int cantidad = sc.nextInt();` |
| 3  | 29    | `error: incompatible types: possible lossy conversion from double to int` | Semántica | Cambiar el tipo de la variable `total` de `int` a `double`: `double total = (base - descuento) * (1 + IVA);` |

- **Cálculo manual frente al resultado obtenido (`precio = 60`, `cantidad = 2`, `socio = s`):**
  - El programa muestra un descuento de `6,00 €` y un total de **`137,94 €`**, pero contiene un error lógico.
  - Calculado a mano con las reglas: `base = 60 × 2 = 120,00 €`. Al superar 100 € corresponde un 10 % (`12,00 €`) más un 5 % por ser socio (`6,00 €`), sumando **`18,00 €`** de descuento. El total con IVA (21 %) debería ser `(120 - 18) × 1,21 =` **`123,42 €`**.

Captura del programa funcionando: capturas/reto2.png

## Reto 3 · Depuración del error lógico
| Variable | Valor esperado | Valor real |
|----------|----------------|------------|
| descuento | 18,00 €        | 6,00 €     |
| total     | 123,42 €       | 137,94 €   |

- Línea del error y explicación:
  - **Línea 27:** `descuento = base * 0.05;`
  - **Explicación:** Al entrar en el segundo `if` (`socio.equals("s")`), se reasignaba el valor de `descuento` directamente en vez de sumarlo al descuento previo. Con ello se perdía el descuento del 10 % (12,00 €) calculado en el primer `if` y solo se aplicaba el 5 % de socio (6,00 €), incumpliendo la regla que exige que ambos descuentos se sumen (15 % total).
- Corrección:
  - Se cambió la asignación directa por suma y asignación (`+=`):
    ```java
    if (socio.equals("s")) {
        descuento += base * 0.05;
    }
    ```
- Mi propia prueba (producto, precio, cantidad, socio → total):
  - **Producto:** Monitor
  - **Precio unitario:** 40,00 €
  - **Cantidad:** 5
  - **Socio:** s
  - **Desglose:** Base = 200,00 €; Descuento = 30,00 € (20,00 € por superar 100 € + 10,00 € por socio); **Total con IVA (21 %): 205,70 €**.

Captura del depurador: capturas/reto3.png

## Reto 4 · Reutilización
- Métodos creados y para qué sirve cada uno:
- ¿Qué ventaja tiene frente al código anterior?

## Reto 5 · ¿Qué lenguaje elegirías?
- Caso A (web de la tienda):
- Caso B (propuesta en ensamblador):
