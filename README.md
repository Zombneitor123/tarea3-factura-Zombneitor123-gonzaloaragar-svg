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
  - **`static double calcularDescuento(double base, boolean socio)`**: Recibe el importe base de la compra y un valor booleano que indica si el cliente es socio. Calcula y devuelve el descuento total acumulado (10 % si la base supera 100 € y un 5 % adicional si es socio).
  - **`static double aplicarIVA(double importe)`**: Recibe el importe neto (la base menos el descuento) y devuelve el total final tras sumarle el 21 % de IVA (`importe * (1 + IVA)`).
  - **`static double pedirDecimal(Scanner sc, String mensaje)`** *(opcional)*: Muestra por pantalla el mensaje indicado y lee un número decimal mediante `sc.nextDouble()`, evitando repetir la combinación `print` + `nextDouble`.
- ¿Qué ventaja tiene frente al código anterior?
  - **Modularidad y legibilidad:** El método `main` queda mucho más limpio y fácil de entender, ya que solo coordina la entrada de datos, las llamadas a las funciones y la salida por pantalla.
  - **Reutilización sin duplicar código:** Si necesitamos calcular descuentos, aplicar el IVA o pedir decimales en varias partes del programa (por ejemplo, para procesar varios productos o facturas), basta con invocar al método sin copiar y pegar las fórmulas.
  - **Mantenibilidad y facilidad para depurar:** Si en el futuro cambian las reglas de negocio (como el porcentaje de descuento o el tipo de IVA), solo hay que modificar el método correspondiente en un único lugar, reduciendo el riesgo de introducir errores lógicos.

## Reto 5 · ¿Qué lenguaje elegirías?
- Caso A (web de la tienda):
  - **Tipo de lenguaje:** **Interpretado** (específicamente **JavaScript** o TypeScript transpilado a JavaScript).
  - **Justificación:**
    1. **Tipo de proyecto y entorno de ejecución:** El objetivo es que la aplicación corra directamente en el navegador web del cliente. JavaScript es el estándar universal e interpretado nativamente por todos los navegadores web (Chrome, Firefox, Safari, Edge) sin requerir instalación de entornos, plugins ni máquinas virtuales adicionales por parte del usuario final.
    2. **Portabilidad:** Máxima portabilidad multiplataforma. El mismo código fuente se ejecuta de forma transparente en cualquier dispositivo (ordenadores de escritorio, portátiles, tablets o smartphones) y bajo cualquier sistema operativo (Windows, macOS, Linux, Android, iOS) siempre que cuente con un navegador web moderno.
    3. **Coste y tiempo de desarrollo:** El desarrollo de formularios dinámicos y cálculo en tiempo real en la web es rápido, económico y sencillo de mantener gracias a la integración nativa con el DOM (HTML/CSS) y el amplio catálogo de librerías disponibles.
    4. **Rendimiento:** Para una calculadora de facturación, la carga de procesamiento es insignificante (se resuelve en microsegundos). No se requiere computación de alto rendimiento ni tiempo real crítico, por lo que los motores de los navegadores (como V8) ofrecen una velocidad óptima e instantánea para la experiencia de usuario.

- Caso B (propuesta en ensamblador):
  - **Tipo de lenguaje:** **Lenguaje de alto nivel mixto (como Java)** o compilado, **rechazando tajantemente el uso de ensamblador**.
  - **Justificación y refutación de la propuesta:**
    1. **Portabilidad (premisa del compañero falsa):** La afirmación de que irá más rápido «en cualquier ordenador» ignora que el lenguaje ensamblador **no es portable**. Está directamente ligado a una arquitectura física de procesador concreta (x86, x86_64, ARM, etc.) y a las llamadas al sistema del SO. Un ejecutable en ensamblador para Intel/AMD x86 no funciona en procesadores ARM (como móviles o Apple Silicon). En cambio, un lenguaje mixto como **Java** garantiza portabilidad total («Write Once, Run Anywhere») mediante la JVM y el bytecode.
    2. **Coste y tiempo de desarrollo:** Programar en ensamblador implica gestionar registros, memoria y llamadas al sistema a muy bajo nivel. Desarrollar la entrada/salida de datos por teclado, el formateo de números decimales y las operaciones aritméticas en ensamblador dispararía los costes, multiplicaría el tiempo de desarrollo y dificultaría enormemente el mantenimiento y la depuración del código.
    3. **Rendimiento o tiempo real:** El cuello de botella de esta calculadora es el tiempo de respuesta humano (esperar a que el usuario introduzca los datos por teclado). La hipotética ganancia de unos pocos ciclos de reloj al calcular el 10 % o el 21 % de IVA es imperceptible para un ser humano (del orden de nanosegundos). El ensamblador solo tiene sentido en sistemas embebidos de recursos extremadamente limitados o controladores de hardware en tiempo real crítico, no en aplicaciones de gestión comercial.
    4. **Tipo de proyecto:** Para una aplicación empresarial o de gestión comercial, las prioridades son la legibilidad, la robustez, la facilidad de mantenimiento y la seguridad de tipos, aspectos donde los lenguajes de alto nivel son infinitamente superiores.

---
## Entrega
- **URL del repositorio:** https://github.com/Zombneitor123/tarea3-factura-Zombneitor123-gonzaloaragar-svg
- **Miembros del equipo:** Manuel Bono Torres (`Zombneitor123`) y Gonzalo Aragón García (`gonzaloaragar-svg`)

