[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/iVzT1xMN)
# Laboratorio: El Proceso de Compilación en C

**Materia:** Sintaxis y Semántica de los Lenguajes (UTN-FRBA)

**Tema:** Proceso de compilación: Del código fuente al ejecutable

---

## Antes de empezar: ¿Cómo funciona este laboratorio?

Este laboratorio se entrega a través de **GitHub Classroom**. Al aceptar el assignment se te creó un repositorio personal en la organización de la materia con todos los archivos necesarios.

**El flujo es simple**: trabajás en `main`, verificás localmente con `make test`, y cuando está listo hacés push. GitHub ejecuta automáticamente el workflow de corrección y podés ver los resultados en la pestaña **Actions** de tu repositorio.

**Opciones para trabajar:**
- **Local:** clonar el repositorio en tu máquina con `git clone` y trabajar con el editor que prefieras.
- **GitHub Codespaces:** desde la página de tu repositorio, hacé clic en el botón verde **"Code" → "Codespaces" → "Create codespace"**. Se abre un VSCode en el navegador con todo el entorno listo, sin instalar nada.

### Paso a paso

#### Paso 1: Clonar el repositorio

En la página de tu repositorio en GitHub, hacé clic en **"Code"** y copiá la URL. Luego en la terminal:

```bash
git clone <URL-de-tu-repositorio>
cd <nombre-de-la-carpeta>
```

Verificar que están los archivos:

```bash
ls
# proceso_compilacion.md  matematica.c  matematica.h  programa.c
```

> **Alternativa sin instalar nada:** desde la misma página, **"Code" → "Codespaces" → "Create codespace"** abre un VSCode en el navegador con el repositorio listo.

#### Paso 2: Ejecutar cada etapa del laboratorio

Seguir la guía del laboratorio ejecutando los comandos de cada etapa. Algunos resultados hay que guardarlos en archivos para que el autograding pueda verificarlos. Antes de guardar las salidas, crear la carpeta donde van a estar:

```bash
mkdir salidas
```

Esto crea una carpeta `salidas/` en el repositorio. Los tres archivos `.txt` que genera el laboratorio (`nm_programa_o.txt`, `nm_ejecutable.txt`, `salida_debug.txt`) van ahí, usando el operador `>` que redirige la salida de un comando a un archivo en lugar de mostrarla en pantalla:

```bash
nm programa.o > salidas/nm_programa_o.txt       # guarda la tabla de símbolos de programa.o
nm programa   > salidas/nm_ejecutable.txt        # guarda la tabla de símbolos del ejecutable
./programa_debug > salidas/salida_debug.txt      # guarda la salida del programa con DEBUG
```

#### Paso 3: Completar las respuestas en `proceso_compilacion.md`

Abrí el archivo con cualquier editor y respondé cada pregunta en el lugar indicado, con la salida real de los comandos que ejecutaste.

#### Paso 4: Verificar localmente y hacer push

Antes de pushear, verificá tu puntaje con:

```bash
make test
```

**Flujo recomendado:** hacé commits frecuentes mientras avanzás, usá `make test` para verificar tu progreso, y dejá el push para cuando una parte esté realmente lista.

Cuando estés listo/a, commiteá y pusheá:

#### Paso 4b: Commitear y hacer push

Verificar qué archivos están listos para commitear:

```bash
git status
```

Agregar los archivos y crear el commit:

```bash
git add programa.i programa.s salidas/ proceso_compilacion.md
git commit -m "Entrega laboratorio proceso de compilación"
git push
```

Verificar que el push fue exitoso:

```bash
git log --oneline
```

> **`programa.o` y los ejecutables no deben aparecer en `git status`.** Si aparecen, el `.gitignore` no está funcionando, no usar `git add .`, agregar cada archivo por nombre.

#### Paso 5: Ver resultados en GitHub

Una vez que hiciste push, entrá a tu repositorio en GitHub:

1. Hacé click en la pestaña **Actions**
2. Hacé click en la ejecución más reciente → job **Autograding**
3. Al final del job vas a ver la tabla con el resultado de cada check y el puntaje total

Si algún check falla, corregí el problema, commiteá y volvé a pushear — el workflow se re-ejecuta solo.

> ⚠️ **Evitá pushes innecesarios.** Cada ejecución consume cómputo en servidores de GitHub — un recurso compartido. El workflow solo se activa cuando pusheás cambios en archivos `.i`, `.s`, `.c`, `salidas/` o `proceso_compilacion.md`. Para el resto, `make test` te da el mismo resultado en tu terminal sin costo.

---

### Qué verifican los checks automáticos

Los checks son de dos tipos:

**Sobre las respuestas en este archivo:**

- Las **respuestas cerradas** (`CLAVE=valor`) se verifican automáticamente con cada push. Deben escribirse exactamente como se indica: sin espacios, sin comillas, respetando mayúsculas.
- Las **respuestas abiertas** (`> **R:**`) no se evalúan automáticamente. Escribilas con tus propias palabras — el docente las revisa manualmente.

---

## Requisitos previos: Instalación de herramientas

Para realizar este laboratorio se necesitan dos herramientas: **GCC** (compilador de C) y **Clang** (compilador alternativo que usaremos para inspeccionar fases internas de la etapa de análisis). Seguir las instrucciones según el sistema operativo (SO) en el que se encuentren.

### Windows

**Instalar GCC mediante MSYS2**

MSYS2 es un entorno que provee GCC y herramientas Unix en Windows.

1. Descargar el instalador desde: `https://www.msys2.org`
2. Ejecutar el instalador y seguir los pasos (dejar la ruta por defecto `C:\msys64`).
3. Al finalizar, abrir la terminal **MSYS2 UCRT64** (buscarla en el menú Inicio).
4. En esa terminal, ejecutar:

```bash
pacman -Syu
pacman -S mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-clang
```

5. Agregar al PATH de Windows la carpeta `C:\msys64\ucrt64\bin` (Panel de control → Variables de entorno → Path → Nuevo).
6. Abrir una nueva terminal (CMD o PowerShell) y verificar:

```powershell
gcc --version
clang --version
```

**Alternativa: instalar solo Clang con winget** (si GCC ya está instalado por otro medio):

```powershell
winget install LLVM.LLVM
```

---

### macOS

Tanto GCC como Clang se instalan juntos con las **Xcode Command Line Tools**. En una terminal:

```bash
xcode-select --install
```

Aparece un cuadro de diálogo; aceptar la instalación. Cuando termine, verificar:

```bash
gcc --version
clang --version
```

**Alternativa: instalar con Homebrew** (si ya tenés Homebrew instalado o preferís versiones más recientes):

```bash
brew install gcc llvm
```

> En macOS, el comando `gcc` en realidad invoca Clang internamente (Apple reemplazó GCC por Clang). Para este laboratorio no hay diferencia práctica: todos los comandos funcionan igual.

---

### Linux (Ubuntu / Debian y derivados)

```bash
sudo apt update
sudo apt install gcc clang
```

Verificar:

```bash
gcc --version
clang --version
```

### Linux (Fedora / RHEL / Rocky)

```bash
sudo dnf install gcc clang
```

---

### Verificación final (todos los sistemas)

Ejecutar estos tres comandos. Si todos muestran un número de versión, el entorno está listo:

```bash
gcc --version
clang --version
nm --version        # en macOS puede decir "Apple LLVM" — está bien
```

---

## Introducción

Cuando escribimos un programa en C y lo "compilamos", en realidad ocurren **cuatro etapas distintas y separadas**. Cada una toma como entrada el resultado de la anterior y produce una representación diferente del programa:

```
programa.c
    │
    ▼ [1] PREPROCESAMIENTO      gcc -E
programa.i  (código C expandido)
    │
    ▼ [2] COMPILACIÓN           gcc -S
programa.s  (código ensamblador)
    │
    ▼ [3] ENSAMBLADO            gcc -c
programa.o  (código objeto binario)
    │
    ▼ [4] ENLAZADO              gcc (ld)
programa    (ejecutable final)
```

Este laboratorio recorre cada etapa de forma práctica usando los archivos fuente provistos.

---

## Conceptos previos

Antes de empezar es importante tener claros tres conceptos que aparecen a lo largo del laboratorio.

### ¿Qué es una directiva?

Una **directiva** es una instrucción dirigida al **preprocesador**, no al compilador. Se reconocen porque comienzan con `#`. No son código C: son órdenes de procesamiento de texto que se ejecutan **antes** de que el compilador vea el código.

```c
#include <stdio.h>      // directiva: "incluir este archivo aquí"
#define PI 3.14159      // directiva: "reemplazar PI por 3.14159 en todo el texto"
#ifdef DEBUG            // directiva: "incluir este bloque sólo si DEBUG está definido"
```

Las directivas no generan código de máquina; desaparecen antes de la compilación.

### ¿Qué es una macro?

Una **macro** es un nombre definido con `#define` que el preprocesador reemplaza textualmente en el código fuente. Hay dos tipos:

- **Macro de objeto** (constante simbólica): reemplaza un nombre por un valor.
  ```c
  #define PI    3.14159     // PI se reemplaza por 3.14159
  #define LIMITE 5          // LIMITE se reemplaza por 5
  ```

- **Macro función**: reemplaza un nombre seguido de argumentos por una expresión.
  ```c
  #define CUADRADO(x)  ((x) * (x))   // CUADRADO(3) → ((3) * (3))
  #define MAX(a, b)    ((a) > (b) ? (a) : (b))
  ```

La diferencia fundamental con una función real es que **la macro se expande en tiempo de preprocesamiento**: el compilador nunca la ve, sólo ve el código ya expandido. Esto tiene ventajas (sin overhead de llamada a función) y riesgos (sin verificación de tipos).

### ¿Qué es una llamada a función externa?

Una **llamada a función externa** ocurre cuando el código de un archivo `.c` llama a una función que **no está definida en ese mismo archivo**. Por ejemplo, en `programa.c`:

```c
area_circulo(radio);   // definida en matematica.c → externa
printf("...");         // definida en libc          → externa
```

Durante la compilación, el compilador sólo necesita saber la **declaración** (firma) de esas funciones (qué parámetros reciben y qué devuelven) para verificar que se las llama correctamente. La **definición** real (el cuerpo de la función) se buscará más tarde, en la etapa de enlazado.

---

## Archivos del laboratorio

| Archivo | Rol |
|---|---|
| `programa.c` | Archivo fuente principal. Contiene `main()`, usa macros, directivas y llama a funciones externas. |
| `matematica.h` | Encabezado con macros (`PI`, `CUADRADO`, `MAX`) y prototipos de funciones. |
| `matematica.c` | Implementación de `area_circulo()` y `factorial()`. Es una **unidad de traducción independiente**. |

> **Unidad de traducción:** cada archivo `.c` (con sus headers incluidos) que el compilador procesa de forma independiente. El enlazador luego las une.

---

## Etapa 1: Preprocesamiento

### Concepto teórico

El **preprocesador** es un paso anterior a la compilación propiamente dicha. No entiende C: trabaja puramente como un procesador de texto que sigue directivas que comienzan con `#`.

Sus tareas son:

| Directiva | Tarea |
|---|---|
| `#include <archivo>` | Busca el archivo **solo en los directorios del sistema** (headers estándar: `stdio.h`, `stdlib.h`, etc.) |
| `#include "archivo"` | Busca **primero en el directorio del proyecto**, y si no lo encuentra, en los del sistema |
| `#define NOMBRE valor` | Define una macro: toda ocurrencia de `NOMBRE` se reemplaza por `valor` |
| `#define MACRO(x) expr` | Define una macro función: se expande con sustitución de parámetros |
| `#ifdef / #ifndef / #endif` | Inclusión condicional de bloques de código |
| Comentarios `/* */` y `//` | Se **eliminan** completamente |

> **`<>` vs `""`:** la diferencia es dónde busca el preprocesador el archivo.
>
> - `#include <stdio.h>`, ángulos: solo busca en los directorios de instalación del compilador (`/usr/include`, etc.). Se usa para headers de la biblioteca estándar o bibliotecas instaladas en el sistema.
> - `#include "matematica.h"`, comillas: busca primero en la carpeta del archivo fuente actual. Si no lo encuentra ahí, recurre a los mismos directorios del sistema. Se usa para headers propios del proyecto.
>
> Regla práctica: **`<>` para lo del sistema, `""` para lo tuyo**.

El resultado es un archivo `.i` con código C "puro": sin directivas, sin comentarios, con todas las macros ya expandidas.

### Práctica

Ejecutar sólo el preprocesador sobre `programa.c`:

```bash
gcc -E programa.c -o programa.i
```

Abrir el archivo generado y observar:

```bash
# Ver cuántas líneas tiene (eran 94 en el fuente original)
wc -l programa.i
```

El número de líneas varía según el sistema operativo y la versión del compilador, porque cada sistema tiene su propia implementación de los headers estándar (`stdio.h`, `stdlib.h`, etc.) con distinta cantidad de declaraciones internas, typedefs y macros auxiliares.

Valores típicos orientativos:
- **macOS** (Xcode / Apple Clang): ~1800 líneas
- **Linux** (Ubuntu/Debian con glibc): ~800–1000 líneas
- **Windows** (MSYS2 / MinGW): ~600–900 líneas

Todos son correctos. Lo importante no es el número exacto sino que sea **varios cientos de veces más** que las ~70 líneas del fuente original, evidencia directa de que `#include` copia el contenido completo de los headers del sistema.

---

**P1.** Ejecutá `wc -l programa.i` y escribí el número de líneas que obtenés.

<!-- Completá la línea siguiente con el número exacto (solo dígitos, sin espacios): -->
LINEAS_I=1822

¿Por qué ese número es tan mayor que las 94 líneas de `programa.c`?

> **R:** El archivo es mas grande porque el preprocesador expande todas las directivas #include, insertando el contenido de las bibliotecas estandar y de usuario. Ademas, estos archivos pueden incluir otros headers de forma recursiva, lo que aumenta considerablemente la cantidad de lineas, junto con la expansion de macros.

---

#### Herramienta: `grep`

`grep` (Global Regular Expression Print) busca líneas que coincidan con un patrón dentro de uno o varios archivos. Su forma básica es:

```bash
grep "patrón" archivo
```

Si no encuentra nada, no imprime nada. Si el patrón no existe en el archivo, el comando termina en silencio. Se usará a lo largo del laboratorio para verificar qué contiene (o no contiene) cada producto intermedio.

Algunas opciones útiles:
- `-n` muestra el número de línea de cada coincidencia
- `-c` cuenta cuántas líneas coinciden (sin mostrarlas)
- `\|` en el patrón significa "o" (busca una cosa o la otra)

#### Observación 1: Los comentarios desaparecen

El encabezado de `programa.c` tenía este comentario de bloque:
```c
/*
 * programa.c  —  Archivo fuente principal
 * ...
 */
```
En `programa.i` no existe ningún rastro de él. Buscar para confirmar:
```bash
grep "Archivo fuente principal" programa.i   # no debe encontrar nada
```

---

**P2.** Ejecutá `grep "Archivo fuente principal" programa.i`.
¿El comando encuentra algo o no devuelve nada?

<!-- Completá con SI (si encontró algo) o NO (si no encontró nada): -->
COMENTARIOS_EN_I=NO

¿Por qué ocurre eso?

> **R:** Porque no hubo una coincidencia de patron, ya que la linea "Archivo fuente principal" no existe en programa.i, por lo tanto, no se imprime nada. 

---

#### Observación 2: Las macros se expanden

En el fuente original:
```c
printf("=== Laboratorio de Compilacion en C (v%s) ===\n\n", VERSION);
printf("CUADRADO(%d)      = %d\n", LIMITE, CUADRADO(LIMITE));
```

En `programa.i` esas líneas quedaron como:
```c
printf("=== Laboratorio de Compilacion en C (v%s) ===\n\n", "1.0");
printf("CUADRADO(%d)      = %d\n", 5, ((5) * (5)));
```

Buscar en el archivo para verificarlo:
```bash
grep -n "1.0\|CUADRADO" programa.i
```

Nótese que `CUADRADO(5)` se expande a `((5) * (5))`, con los paréntesis extra que evitan errores de precedencia de operadores. Si la macro fuera `#define CUADRADO(x) x * x` (sin paréntesis), entonces `CUADRADO(1+2)` daría `1+2 * 1+2 = 5` en lugar de `9`.

---

**P3.** Ejecutá `grep -n "CUADRADO" programa.i` y copiá la salida completa.

> **R:** 1791:    printf("CUADRADO(%d)      = %d\n", 5, ((5) * (5)));

¿El nombre `CUADRADO` aparece tal cual en `programa.i`, o fue reemplazado
por otra cosa? Respondé SI o NO:

<!-- Completá con SI o NO: -->
CUADRADO_EN_I=NO

---

**P4.** Ejecutá `grep -n '"1\.0"' programa.i` y copiá la línea encontrada.

> **R:** 1782:    printf("=== Laboratorio de Compilacion en C (v%s) ===\n\n", "1.0");

¿Cuál era el nombre de la macro en `programa.c` que fue reemplazada por `"1.0"`?

<!-- Completá con el nombre exacto de la macro (en mayúsculas, como está en el fuente): -->
NOMBRE_MACRO_VERSION=VERSION

---

#### Observación 3: Compilación condicional

`programa.c` tiene este bloque:
```c
#ifdef DEBUG
    #define LOG(msg)  printf("[DEBUG] %s\n", (msg))
#else
    #define LOG(msg)  /* vacío */
#endif
```

Sin la bandera `-DDEBUG`, la macro `LOG` se define como vacía, así que la línea `LOG("Iniciando main")` desaparece del código:

```bash
# Sin DEBUG: LOG no genera código
gcc -E programa.c | grep "Iniciando"   # no encuentra nada

# Con DEBUG: LOG se expande a printf
gcc -E -DDEBUG programa.c | grep "Iniciando"
# Resultado: printf("[DEBUG] %s\n", ("Iniciando main"));
```

> **Conclusión:** la compilación condicional permite incluir o excluir código en tiempo de preprocesamiento, **antes** de que el compilador analice el código.

---

**P5.** Ejecutá los siguientes dos comandos y copiá la salida de cada uno:
```bash
gcc -E programa.c | grep "Iniciando"
gcc -E -DDEBUG programa.c | grep "Iniciando"
```

> **R:** 
> ```bash 
>       gcc -E programa.c | grep "Iniciando" 
>          
>       gcc -E -DDEBUG programa.c | grep "Iniciando" 
>       printf("[DEBUG] %s\n", ("Iniciando main"));
> ```

¿Agregar `-DDEBUG` hace que aparezca código nuevo en el `.i` que antes no estaba?
Respondé SI o NO:

<!-- Completá con SI o NO: -->
DEBUG_ACTIVA_CODIGO=SI

---

**P6.** En `programa.i` vas a encontrar líneas con este formato intercaladas entre el código C:

```
# 1 "<built-in>"
# 1 "/usr/include/stdio.h" 1 3 4
# 412 "/usr/include/stdio.h" 3 4
# 1 "programa.c"
```

Son **marcadores de línea** que el preprocesador inserta para registrar el origen de cada bloque. El formato es `# <número_de_línea> "<archivo>"`. El compilador los usa para reportar errores con el nombre y línea del archivo original (no del `.i`), y el debugger los usa para mapear instrucciones de máquina al fuente C.

Ejecutá este comando para ver de dónde viene la declaración de `printf`:

```bash
grep -n "stdio.h" programa.i | head -5
```

¿Qué información comunican esas líneas `# N "archivo"`? ¿De qué archivo proviene el bloque que contiene la declaración de `printf`?

> **R:** Indican el número de línea y el archivo de origen del código que sigue. El preprocesador las inserta para que el compilador y el debugger puedan asociar cada parte del código generado con su archivo y línea original, osea si ocurre un error o si se está usando un debugger, el sistema puede indicarnos la ubicación en nuestro código fuente original (o en las bibliotecas) y no en el archivo **.i**. El bloque que contiene la declaración de printf proviene del archivo de cabecera estándar stdio.h.

---

## Etapa 2: Compilación (Análisis + Generación de código)

### Concepto teórico

Esta etapa es la que suele llamarse "compilación" en sentido estricto. Toma el código C preprocesado y lo traduce a **lenguaje ensamblador**. Internamente se divide en fases:

```
Código C preprocesado
        │
        ▼
   ┌─────────────────┐
   │ Análisis Léxico │  Identifica tokens: palabras clave, identificadores,
   │  (Scanner)      │  literales, operadores, delimitadores.
   └────────┬────────┘
            │ flujo de tokens
            ▼
   ┌──────────────────┐
   │ Análisis         │  Verifica que los tokens forman estructuras
   │ Sintáctico       │  gramaticalmente correctas (construye el AST:
   │  (Parser)        │  Árbol de Sintaxis Abstracta).
   └────────┬─────────┘
            │ AST
            ▼
   ┌──────────────────┐
   │ Análisis         │  Verifica tipos, declaraciones previas,
   │ Semántico        │  compatibilidad de operaciones.
   └────────┬─────────┘
            │ AST anotado
            ▼
   ┌──────────────────┐
   │ Generación de    │  Produce código en lenguaje ensamblador
   │ Código           │  (dependiente de la arquitectura del procesador).
   └──────────────────┘
```

### Las fases internas del compilador

El diagrama resume cuatro fases. Acá se explica qué hace concretamente cada una.

#### Análisis léxico (scanner)

El código fuente preprocesado es, a los ojos del compilador, una secuencia de caracteres sin estructura. La primera tarea es dividir esa cadena de caracteres en **tokens**: las unidades atómicas con significado del lenguaje.

Un **token** tiene dos atributos: su **clase** (qué tipo de cosa es) y su **lexema** (el texto exacto en el fuente). Las clases de tokens en C son:

| Clase | Ejemplos |
|---|---|
| Palabra clave (keyword) | `int`, `return`, `if`, `while`, `void` |
| Identificador | `sumar`, `resultado`, `llamadas`, `main` |
| Literal numérico | `3`, `4`, `5.0`, `3.14` |
| Literal de cadena | `"Laboratorio"`, `"%d\n"` |
| Operador | `+`, `++`, `=`, `>`, `*` |
| Delimitador / puntuador | `(`, `)`, `{`, `}`, `;`, `,` |

El analizador léxico **no sabe nada de gramática**: no distingue si un `(` abre la firma de una función o la condición de un `if`. Sólo reconoce patrones usando expresiones regulares (o autómatas finitos equivalentes). Su salida es un flujo lineal de tokens que pasa a la siguiente fase.

Un error léxico ocurre cuando el scanner encuentra una secuencia de caracteres que no forma ningún token válido del lenguaje (por ejemplo, un carácter `@` suelto en C, o una cadena sin cerrar).

#### Análisis sintáctico (parser)

El parser toma el flujo de tokens y verifica que su **orden y estructura** respetan la gramática del lenguaje. El resultado es el **AST** (ver más abajo). Si los tokens están en un orden gramaticalmente inválido (falta un `;`, un `{` sin cerrar, etc.), se produce un error sintáctico.

#### Análisis semántico

Con el árbol construido, el análisis semántico verifica el **significado** de las construcciones: que los tipos sean compatibles en una asignación, que una función se llame con la cantidad correcta de argumentos, que una variable esté declarada antes de usarse. Es la fase que detecta errores que son gramaticalmente correctos pero sin sentido, como sumar un entero y una cadena de texto.

#### ¿Qué es el AST?

El **AST** (Abstract Syntax Tree, Árbol de Sintaxis Abstracta) es la estructura de datos central del compilador: una representación jerárquica en forma de árbol del programa. Cada nodo del árbol representa una construcción del lenguaje.

Se llama "abstracto" porque **omite detalles sintácticos superfluos** que no aportan significado: los paréntesis de agrupación, los puntos y coma, las llaves. Lo que importa es la estructura lógica, no los tokens exactos que la delimitaron.

Por ejemplo, el código fuente:
```c
int sumar(int a, int b) {
    llamadas++;
    return a + b;
}
```
produce un árbol como éste:
```
FunctionDecl  "sumar"  tipo: int(int, int)
├── ParmVarDecl  "a"  tipo: int
├── ParmVarDecl  "b"  tipo: int
└── CompoundStmt  (el cuerpo {})
    ├── UnaryOperator  "++"  (postfijo)
    │   └── DeclRefExpr  "llamadas"
    └── ReturnStmt
        └── BinaryOperator  "+"
            ├── DeclRefExpr  "a"
            └── DeclRefExpr  "b"
```

Los nodos hoja son los valores concretos; los nodos internos son las operaciones o estructuras que los combinan. El análisis semántico anota cada nodo con información de tipos, y luego la generación de código recorre el árbol para producir instrucciones ensamblador.

---

### ¿Qué es el lenguaje ensamblador y qué es un archivo `.s`?

El **lenguaje ensamblador** (assembly) es la representación textual y legible de las instrucciones de máquina de un procesador. Cada instrucción corresponde directamente a una operación elemental del procesador: mover un valor de memoria a un registro, sumar dos registros, saltar a otra posición del programa, etc.

Es el nivel de abstracción más bajo que aún puede leer un humano. A diferencia de C, no hay bucles `for`, funciones con nombre propio ni tipos: todo son registros (`w0`, `x1`...), posiciones de memoria y saltos condicionales.

Un archivo `.s` es simplemente texto con esas instrucciones. Cada arquitectura tiene su propio conjunto de instrucciones (ISA):
- **ARM64** (Apple Silicon, Raspberry Pi): instrucciones como `sub`, `str`, `ldr`, `bl`, `ret`
- **x86-64** (Intel/AMD en Linux y Windows): instrucciones como `mov`, `push`, `call`, `ret`

El código generado es equivalente en ambas, sólo cambia el vocabulario.

### Práctica

```bash
gcc -S programa.c -o programa.s
```

Abrir `programa.s` y explorar su contenido. Es texto legible, aunque de más bajo nivel que C.

```bash
# Ver las primeras líneas
head -25 programa.s
```

Se verá algo como (en arquitectura ARM64 / Apple Silicon):

```asm
    .globl  _sumar              ; símbolo exportado: es visible para el enlazador
_sumar:                         ; etiqueta: marca dónde comienza la función sumar
    sub     sp, sp, #16         ; reserva 16 bytes en la pila (stack)
    str     w0, [sp, #12]       ; guarda el parámetro 'a' en la pila
    str     w1, [sp, #8]        ; guarda el parámetro 'b' en la pila
    ...
    ldr     w8, [sp, #12]       ; carga 'a' en el registro w8
    ldr     w9, [sp, #8]        ; carga 'b' en el registro w9
    add     w0, w8, w9          ; w0 = w8 + w9  (w0 es el registro de retorno)
    ret                         ; retorna al llamador
```

> En x86-64 (Linux/Windows) el ensamblador generado tendrá instrucciones distintas (`mov`, `add`, `ret`) pero el concepto es el mismo.

#### Observación: Las funciones externas aparecen como llamadas sin definición

Buscar referencias a funciones externas en el ensamblador:
```bash
grep "area_circulo\|factorial\|printf" programa.s
```

Aparecen como instrucciones de llamada (por ejemplo `bl _area_circulo`), pero **no existe ningún bloque `_area_circulo:` con su código**. El compilador sabe que esas funciones existen (las declaró el encabezado), pero no tiene su cuerpo, eso lo resuelve el enlazador.

---

**P7.** Ejecutá `grep "area_circulo" programa.s` y copiá la salida.

> **R:** 
> ```bash
>   .ascii "area_circulo(%.1f) = %.4f\12\0" 
>   call    area_circulo 
>   .def    area_circulo;   .scl    2;      .type   32;     .endef
>```

¿`area_circulo` aparece como una función *definida* en `programa.s`
(con su propio bloque de instrucciones) o solo como una *llamada* (instrucción sin cuerpo)?
Respondé DEFINIDA o LLAMADA:

<!-- Completá con DEFINIDA o LLAMADA: -->
AREA_EN_S=LLAMADA

---

**P8.** Encontrá en `programa.s` la etiqueta `sumar:` o `_sumar:` y copiá
las primeras 4 líneas de instrucciones que le siguen.

> **R:** 
> ```asm
>   pushq	%rbp
>   .seh_pushreg	%rbp
>   movq	%rsp, %rbp
>   .seh_setframe	%rbp, 0
>```

Explicá en términos generales qué hacen esas instrucciones
(usá los comentarios del laboratorio como guía):

> **R:** 
> ```asm
>   pushq	%rbp                ;   resguarda el valor del puntero base anterior en la pila 
>   .seh_pushreg	%rbp        ;   registra el resguardo de %rbp para el manejo de excepciones
>   movq	%rsp, %rbp          ;   establece un nuevo marco de pila (puntero base = puntero de pila actual)
>   .seh_setframe	%rbp, 0     ;   informa al sistema operativo que %rbp es el nuevo marco de referencia
>```
---

Ejecutar para verificar si la variable `llamadas` aparece en el ensamblador:
```bash
grep "llamadas" programa.s
```

---

**P9.** Ejecutá `grep "llamadas" programa.s` y copiá la salida.

> **R:** 
> ```bash
>        .globl  llamadas
> llamadas:
>        movl    llamadas(%rip), %eax
>        movl    %eax, llamadas(%rip)
>        movl    llamadas(%rip), %eax
>```

¿Aparece la variable `llamadas` en el ensamblador?
Respondé SI o NO:

<!-- Completá con SI o NO: -->
LLAMADAS_EN_S=SI

---

#### Errores detectados en esta etapa

Introducir deliberadamente un error semántico. Editar `programa.c` y cambiar:
```c
resultado = sumar(3, 4);
```
por:
```c
resultado = sumar(3, 4, 5);   /* demasiados argumentos */
```
Al intentar compilar:
```bash
gcc -S programa.c   # genera un error de tipos/argumentos
```
El compilador lo detecta porque ya tiene la declaración de `sumar` y puede verificar la firma. **Restaurar** el código original antes de continuar.

---

## Etapa 3: Ensamblado

### Concepto teórico

El **ensamblador** (`as`) traduce el archivo `.s` (texto) a **código objeto** (binario). El resultado es un archivo `.o` en un formato binario estructurado:

- **ELF** en Linux
- **Mach-O** en macOS
- **COFF/PE** en Windows

Todos estos formatos tienen la misma idea: son contenedores binarios que almacenan varias secciones:

| Sección | Contenido |
|---|---|
| `.text` | Las instrucciones de máquina de las funciones |
| `.data` | Variables globales con valor inicial |
| `.bss` | Variables globales inicializadas en cero |
| Tabla de símbolos | Lista de nombres (funciones/variables) que este archivo define o necesita |
| Tabla de reubicación | Lista de posiciones que el enlazador deberá corregir cuando asigne direcciones finales |

#### ¿Qué son los símbolos indefinidos (`U`)?

Un archivo `.o` puede hacer referencia a funciones o variables que **no están en él**: viven en otro `.o` o en una biblioteca. Esas referencias se registran en la tabla de símbolos con el tipo `U` (Undefined = indefinido), lo que significa: *"sé que esto existe y lo necesito, pero no sé dónde está todavía"*.

Esto es intencional: permite compilar cada archivo `.c` de forma independiente, y luego unir todo en el enlazado. Si los `.o` no pudieran tener referencias indefinidas, habría que compilar todos los archivos juntos siempre, lo que sería muy costoso en proyectos grandes.

#### Herramienta: `nm`

`nm` (Name Map) es una utilidad que **lista la tabla de símbolos** de un archivo objeto o ejecutable. Para cada símbolo muestra:

```
<dirección>  <tipo>  <nombre>
```

Los tipos más comunes son:

| Tipo | Significado |
|---|---|
| `T` | Definido en la sección **text** (código ejecutable), funciones |
| `D` | Definido en la sección **data**, variable global **con valor inicial** (ej: `int x = 5;`) |
| `S` | Definido en una sección de datos del sistema (macOS); similar a `D` pero en una sección separada que el SO gestiona. En Linux esto suele aparecer como `D` |
| `B` | Definido en la sección **BSS** (Block Started by Symbol), variable global **sin valor inicial** o inicializada en cero (ej: `int x;` o `int x = 0;`). El SO reserva el espacio pero no lo guarda en el archivo, solo anota el tamaño |
| `U` | **Indefinido** (Undefined). se necesita pero no está definido en este archivo |

> **`D` vs `B`:** ambos son variables globales, pero `D` tiene un valor inicial que se debe guardar en el archivo objeto (porque el programa necesita leerlo al arrancar), mientras que `B` se inicializa en cero y el sistema operativo puede hacerlo solo al cargar el proceso, no hace falta guardarlo en el archivo. Esto hace que los ejecutables sean más pequeños.

> **`S` en macOS:** en macOS, las variables globales con valor inicial suelen aparecer como `S` en lugar de `D` porque Mach-O (el formato de objeto de macOS) las ubica en una sección `__DATA` separada. El concepto es el mismo que `D`.

`nm` es útil para diagnosticar errores de enlazado: si el enlazador dice *"símbolo no encontrado"*, `nm` permite ver exactamente en qué `.o` se necesita y en cuál debería estar definido.

### Práctica

Generar los archivos objeto de ambas unidades de traducción:

```bash
gcc -c programa.c  -o programa.o
gcc -c matematica.c -o matematica.o
```

#### Observar la tabla de símbolos

La salida de `nm` tiene tres columnas: **dirección**, **tipo** y **nombre**. La dirección es un número hexadecimal de 16 dígitos que indica dónde quedó ubicado ese símbolo en memoria (relativo al inicio del archivo objeto). Los símbolos indefinidos (`U`) no tienen dirección todavía, por eso aparecen en blanco.

```bash
nm programa.o
```

Salida esperada (simplificada):

```
                 U _area_circulo      ← INDEFINIDO: lo define matematica.o
                 U _factorial         ← INDEFINIDO: lo define matematica.o
000000000000018c T _imprimir_separador ← DEFINIDO en este archivo (T = texto/código)
0000000000000300 S _llamadas          ← DEFINIDO: variable global
0000000000000030 T _main              ← DEFINIDO: función main
                 U _printf            ← INDEFINIDO: lo define libc
```

---

**P10.** Ejecutá `nm programa.o` y copiá la salida completa.

> **R:** 
>```bash 
>0000000000000000 b .bss
>0000000000000000 d .data
>0000000000000000 p .pdata
>0000000000000000 r .rdata
>0000000000000000 r .rdata$zzz
>0000000000000000 t .text
>0000000000000000 r .xdata
>                 U __imp___acrt_iob_func
>                 U __main
>                 U __mingw_vfprintf
>                 U area_circulo
>                 U factorial
>00000000000001b3 T imprimir_separador
>0000000000000000 B llamadas
>0000000000000077 T main
>0000000000000000 t printf
>0000000000000054 T sumar
>```

¿Con qué letra aparece `area_circulo` en esa tabla?
Escribí solo la letra (una mayúscula):

<!-- Completá con la letra exacta que muestra nm (U, T, D, etc.): -->
TIPO_AREA_EN_O=U

---

```bash
nm matematica.o
```

```
0000000000000000 T _area_circulo      ← DEFINIDO aquí
0000000000000034 T _factorial         ← DEFINIDO aquí
```

> **Clave:** `programa.o` declara que *necesita* `_area_circulo` y `_factorial` (símbolo `U`). `matematica.o` los *define* (símbolo `T`). El enlazador los conectará.

---

**P11.** ¿Por qué `area_circulo` tiene ese tipo en `programa.o`
pero tipo `T` en `matematica.o`?

> **R:** En programa.o la funcion area_circulo tiene tipo **U (Undefined)** porque en ese archivo solo se encuentra la declaracion y una llamada a la funcion, pero no su implementacion. El compilador sabe que existe, pero no sabe que instrucciones ejecutar.
> En cambio, en matematica.o tiene tipo **T (Text)** porque ahi es donde se encuentra la definicion. El simbolo **T** indica que la funcion esta definida en la sección de codigo de ese archivo objeto.

¿Qué etapa del proceso de compilación resuelve esa diferencia?
Respondé con una palabra: PREPROCESAMIENTO, COMPILACION, ENSAMBLADO o ENLAZADO:

<!-- Completá con una de las cuatro opciones: -->
ETAPA_QUE_RESUELVE=ENLAZADO

---

#### ¿Por qué no se puede ejecutar todavía?

```bash
./programa.o   # Error: no es un ejecutable, es un archivo objeto
```

Un `.o` no es ejecutable por dos razones:

1. Tiene **símbolos indefinidos**, las funciones externas aún no están resueltas.
2. Le falta la **infraestructura de inicio del proceso**: cuando el sistema operativo lanza un ejecutable, no llama directamente a `main()`. Antes ejecuta código de inicialización (`crt0`, "C Runtime 0") que configura el entorno de C (argumentos, variables de entorno, memoria), llama a `main()`, y luego llama a `exit()` con el valor retornado. Ese código lo aporta `libc` durante el enlazado.

---

**P12.** Intentá ejecutar `./programa.o` directamente. ¿Qué mensaje aparece?

> **R:** 
>```bash 
> ./programa.o: cannot execute binary file: Exec format error
>```

¿Se puede ejecutar un archivo `.o` directamente?
Respondé SI o NO:

<!-- Completá con SI o NO: -->
EJECUTABLE_O=NO

---

## Etapa 4 Enlazado

### Concepto teórico

El **enlazador** (linker, `ld`) combina uno o más archivos `.o` y resuelve todos los símbolos indefinidos. Para eso:

1. Reúne todos los archivos objeto especificados.
2. Busca en las **bibliotecas** los símbolos que aún faltan.
3. **Relocaliza** las direcciones: asigna direcciones de memoria definitivas a cada función y variable.
4. Agrega el código de inicio del proceso (`crt0`/`crt1`).
5. Produce el ejecutable final.

```
programa.o  ──┐
              ├──► enlazador ──► programa (ejecutable)
matematica.o ─┤
              │
libc.dylib  ──┘  (contiene printf, exit, crt1, malloc, etc.)
```

#### ¿Qué es una biblioteca?

Una **biblioteca** (library) es una colección de funciones precompiladas, empaquetadas en un único archivo, listas para ser reutilizadas por cualquier programa. Existen dos tipos:

**Biblioteca estática** (`.a` en Linux/macOS, `.lib` en Windows):
- Es un archivo que contiene varios `.o` agrupados.
- El enlazador **copia** el código de las funciones que se usan directamente dentro del ejecutable.
- El ejecutable resultante no tiene dependencias externas en tiempo de ejecución.
- Desventaja: el ejecutable es más grande; si la biblioteca se actualiza, hay que recompilar.

**Biblioteca dinámica** (`.so` en Linux, `.dylib` en macOS, `.dll` en Windows):
- El ejecutable sólo guarda una **referencia** al nombre de la biblioteca.
- El código real se carga en memoria en tiempo de ejecución, cuando se lanza el programa.
- Ventaja: menor tamaño del ejecutable; múltiples programas comparten la misma copia en memoria; actualizar la biblioteca no requiere recompilar los programas.
- Desventaja: si la biblioteca no está instalada o tiene una versión incompatible, el programa falla al iniciar.

La biblioteca estándar de C (**`libc`**) es la biblioteca fundamental de todo programa C. Contiene funciones como `printf`, `malloc`, `free`, `fopen`, `exit`, y cientos más que el programador usa sin pensar de dónde vienen. En Linux se llama `glibc` (GNU C Library); en macOS es parte de `libSystem`.

`libc` existe en ambas formas (estática y dinámica). Por defecto, `gcc` usa la versión dinámica.

Una función especialmente importante de `libc` es **`exit()`**. Cuando `main()` retorna un valor (por ejemplo `return 0;`), no es el sistema operativo quien lo recibe directamente, es el código de inicio del proceso (`crt1`) quien llamó a `main()`, y al recibir el retorno llama a `exit(valor_retornado)`. `exit()` ejecuta los destructores registrados, cierra los buffers de I/O abiertos (por eso `printf` termina de escribir aunque no hayas llamado a `fflush`) y finalmente hace la llamada al sistema `_exit()` que notifica al SO el código de salida. Por convención, `return 0` (o `exit(0)`) indica éxito; cualquier otro valor indica error.

**Enlazado estático vs dinámico:**

| | Estático | Dinámico |
|---|---|---|
| Las funciones de bibliotecas | se copian en el ejecutable | se cargan en tiempo de ejecución |
| Tamaño del ejecutable | mayor | menor |
| Dependencias en tiempo de ejecución | ninguna | necesita las `.so`/`.dylib` |
| Uso típico | sistemas embebidos, distribución única | aplicaciones de escritorio/servidor |

### Práctica

```bash
gcc programa.o matematica.o -o programa
```

`gcc` actúa aquí como un *driver* que invoca al enlazador real (`ld`) con los parámetros correctos, incluyendo automáticamente `libc`.

#### Verificar que los símbolos se resolvieron

```bash
nm programa | grep -E "sumar|factorial|area|main|llamadas|imprimir"
```

Todos los símbolos que antes eran `U` (indefinidos) ahora tienen direcciones concretas:

```
00000001000006a0 T _area_circulo
0000000100000684 T _imprimir_separador
0000000100008000 S _llamadas
0000000100000528 T _main
00000001000004f8 T _sumar
00000001000006d4 T _factorial
```

```bash
nm programa | grep area_circulo
```

---

**P13.** Enlazá con `gcc programa.o matematica.o -o programa`.
Ejecutá `nm programa | grep "area_circulo"` y copiá la salida.

> **R:**

¿Con qué letra aparece ahora `area_circulo` en el ejecutable final?
Escribí solo la letra:

<!-- Completá con la letra exacta que muestra nm: -->
TIPO_AREA_ENLAZADO=

---

#### Verificar qué símbolos siguen siendo indefinidos en el ejecutable

```bash
nm programa | grep "^ *U"
```

Quedan algunos `U` incluso en el ejecutable final. ¿Por qué? Son funciones de la biblioteca dinámica del sistema (`libc.dylib`): como se cargan en tiempo de ejecución, el enlazador no las copia, sólo deja registrado su nombre para que el **cargador dinámico** (`dyld`/`ld.so`) las resuelva cuando el programa se ejecute.

---

**P14.** Ejecutá `nm programa | grep "^ *U"` y copiá la salida.

> **R:**

¿Quedan símbolos de tipo `U` en el ejecutable final?
Respondé SI o NO:

<!-- Completá con SI o NO: -->
SIMBOLOS_U_FINAL=

¿Por qué quedan? ¿Quién los resuelve y cuándo?

> **R:**

---

#### Ejecutar

```bash
./programa
```

---

**P15.** Ejecutá `./programa` y copiá la salida completa.

> **R:**

¿Qué valor da `factorial(5)`? Escribí solo el número:

<!-- Completá con el número exacto: -->
FACTORIAL_5=

---

## Conceptos

---

**P16.** Explicá con tus palabras la diferencia entre una **macro función**
como `CUADRADO(x)` y una **función real** como `sumar(a, b)`.
¿En qué etapa "desaparece" cada una? ¿Cuál tiene verificación de tipos?

> **R:**

---

**P17.** ¿Qué diferencia hay entre un símbolo de tipo `T` y uno de tipo `D`
en la salida de `nm`? ¿En qué sección del archivo objeto vive cada uno?

> **R:**

---

**P18.** (Bonus) Ejecutá `otool -L programa` (macOS) o `ldd programa` (Linux)
y copiá la salida.

> **R:**

¿Por qué `libc` no hubo que especificarla explícitamente al enlazar con `gcc`?

> **R:**

---

## Bonus: inspeccionar las fases internas

### ¿Qué es Clang?

**Clang** es un compilador de C/C++ de código abierto, desarrollado como parte del proyecto **LLVM**. Es el compilador por defecto en macOS (lo instala Xcode Command Line Tools) y compite directamente con GCC en Linux.

Desde el punto de vista del usuario, Clang y GCC son intercambiables: aceptan los mismos flags (`-E`, `-S`, `-c`, `-Wall`, `-DNOMBRE`, etc.) y producen ejecutables equivalentes. La diferencia está en la arquitectura interna: Clang está diseñado con una separación más clara entre fases y expone su funcionamiento interno a través de flags especiales (`-Xclang`), lo que lo hace especialmente útil para estudiar el proceso de compilación.

**LLVM** (Low Level Virtual Machine) es la infraestructura de compilación sobre la que Clang se construye. GCC y Clang son los *frontends* (analizan C y producen una representación intermedia); LLVM es el *backend* (optimiza y genera código de máquina).

```
Código C
   │
   ▼
Clang (frontend)  →  LLVM IR (representación intermedia)  →  LLVM (backend)  →  código máquina
```

Para este laboratorio, lo relevante es que Clang permite observar los tokens y el AST que GCC produce internamente pero no muestra.

---

GCC fusiona internamente el análisis léxico, sintáctico y semántico sin exponer productos intermedios separados. Con Clang podemos hacer visible cada fase.

### Fase léxica: observar los tokens

El analizador léxico convierte la secuencia de caracteres del fuente en una secuencia de tokens. Ejecutar:

```bash
# Mostrar sólo los tokens que provienen de programa.c (no de los headers del sistema)
clang -Xclang -dump-tokens programa.c 2>&1 | grep "programa.c"
```

Cada línea de la salida describe un token con tres datos: su **clase** (qué tipo de token es), su **lexema** (el texto exacto), y su **posición** en el fuente (archivo:línea:columna).

Fragmento de la salida para la función `sumar`:

```
int 'int'          Loc=<programa.c:44:1>   ← palabra clave
identifier 'sumar' Loc=<programa.c:44:5>   ← identificador
l_paren '('        Loc=<programa.c:44:10>  ← delimitador
int 'int'          Loc=<programa.c:44:11>  ← palabra clave
identifier 'a'     Loc=<programa.c:44:15>  ← identificador
comma ','          Loc=<programa.c:44:16>  ← puntuador
...
plusplus '++'      Loc=<programa.c:45:13>  ← operador
return 'return'    Loc=<programa.c:46:5>   ← palabra clave
plus '+'           Loc=<programa.c:46:14>  ← operador
```

Nótese que `int` y `return` son **palabras clave** (reservadas por el lenguaje), mientras que `sumar`, `a`, `b` son **identificadores** (nombres definidos por el programador). El scanner los distingue porque las palabras clave están listadas internamente; cualquier otro nombre válido es un identificador.

También se puede ver que los **comentarios y espacios en blanco no aparecen**: ya fueron eliminados en el preprocesamiento.

> `programa.c` produce ~188 tokens (sin contar los de los headers incluidos). Comparar con las ~1802 líneas del `.i` que incluye todo `stdio.h`.

### Fase sintáctica y semántica: observar el AST

Una vez que el parser tiene la secuencia de tokens, construye el AST verificando que los tokens respetan la gramática de C. El análisis semántico luego anota cada nodo con su tipo.

```bash
# Ver el AST completo de programa.c (salida extensa)
clang -Xclang -ast-dump programa.c 2>/dev/null | grep -A 20 "FunctionDecl.*sumar"
```

Salida real del AST para `sumar`:

```
FunctionDecl sumar 'int (int, int)'      ← función: nombre, tipo de retorno y parámetros
├─ParmVarDecl a 'int'                    ← parámetro a de tipo int
├─ParmVarDecl b 'int'                    ← parámetro b de tipo int
└─CompoundStmt                           ← cuerpo de la función { ... }
  ├─UnaryOperator postfix '++'           ← expresión: llamadas++
  │ └─DeclRefExpr 'llamadas' 'int'       ←   referencia a la variable llamadas (tipo int)
  └─ReturnStmt                           ← sentencia: return ...
    └─BinaryOperator 'int' '+'           ←   operación suma, resultado de tipo int
      ├─ImplicitCastExpr → DeclRefExpr 'a' 'int'   ← operando izquierdo: a
      └─ImplicitCastExpr → DeclRefExpr 'b' 'int'   ← operando derecho: b
```

Puntos clave a observar:

- El **árbol refleja la jerarquía**: la función contiene un bloque, el bloque contiene sentencias, las sentencias contienen expresiones, las expresiones contienen subexpresiones.
- Cada nodo tiene su **tipo anotado** (`'int'`, `'double'`, etc.), esto lo hizo el análisis semántico.
- `ImplicitCastExpr` son conversiones de tipo que el programador no escribió pero el compilador insertó automáticamente. Están en el AST aunque sean invisibles en el código fuente.
- Los **paréntesis, llaves y puntos y coma no aparecen en el AST**: fueron necesarios para parsear la estructura, pero una vez construido el árbol ya no hacen falta. Por eso se llama "abstracto": abstrae los detalles de notación y conserva sólo la estructura semántica.

> **Importante:** GCC y Clang son compiladores de producción, y estas fases ocurren en fracciones de segundo de forma integrada. Lo que importa es entender que **estas fases existen y operan en secuencia**, y que cada una detecta un tipo distinto de error (léxico → sintáctico → semántico).

---

## Flujo completo: Resumen de comandos

```bash
# Todas las etapas de golpe (lo que hace gcc por defecto):
gcc -Wall programa.c matematica.c -o programa

# O paso a paso:
gcc -E  programa.c          -o programa.i    # 1. Preprocesamiento
gcc -S  programa.c          -o programa.s    # 2. Compilación (hasta ensamblador)
gcc -c  programa.c          -o programa.o    # 3. Ensamblado (objeto)
gcc -c  matematica.c        -o matematica.o  # 3. Ensamblado (objeto)
gcc     programa.o matematica.o -o programa  # 4. Enlazado

# Compilar con modo debug activado:
gcc -Wall -DDEBUG programa.c matematica.c -o programa_debug
./programa_debug   # verá los mensajes [DEBUG]

# Inspeccionar fases internas (requiere clang):
clang -Xclang -dump-tokens programa.c 2>&1 | grep "programa.c"   # tokens
clang -Xclang -ast-dump    programa.c 2>/dev/null | grep -A 20 "FunctionDecl.*sumar"  # AST
```

---

## Tabla de extensiones de archivo

| Extensión | Contenido | Etapa que lo produce |
|---|---|---|
| `.c` | Código fuente C | (escrito por el programador) |
| `.h` | Encabezado C | (escrito por el programador) |
| `.i` | C preprocesado (texto) | Preprocesador (`-E`) |
| `.s` | Ensamblador (texto) | Compilador (`-S`) |
| `.o` | Código objeto (binario) | Ensamblador (`-c`) |
| `.a` | Biblioteca estática (colección de `.o`) | Herramienta `ar` |
| `.so` / `.dylib` | Biblioteca dinámica | Enlazador con `-shared` |
| (sin ext. / `.exe`) | Ejecutable (binario) | Enlazador |

---

## Cómo entregar este laboratorio

### Qué hay que commitear

Al finalizar, el repositorio debe contener estos archivos (los fuentes `programa.c`, `matematica.c`, `matematica.h` y `proceso_compilacion.md` ya estaban):

| Archivo | Cómo se genera |
|---|---|
| `programa.i` | `gcc -E programa.c -o programa.i` |
| `programa.s` | `gcc -S programa.c -o programa.s` |
| `salidas/nm_programa_o.txt` | `nm programa.o > salidas/nm_programa_o.txt` |
| `salidas/nm_ejecutable.txt` | `nm programa > salidas/nm_ejecutable.txt` |
| `salidas/salida_debug.txt` | `gcc -DDEBUG programa.c matematica.c -o programa_debug && ./programa_debug > salidas/salida_debug.txt` |
| `proceso_compilacion.md` | completado con tus respuestas |

**No commitear:** `programa.o`, `matematica.o`, ni ejecutables. El `.gitignore` ya los excluye.

---

## Referencia rápida: Opciones de GCC usadas

| Opción | Efecto |
|---|---|
| `-E` | Detener tras el preprocesamiento |
| `-S` | Detener tras la compilación (genera `.s`) |
| `-c` | Detener tras el ensamblado (genera `.o`) |
| `-o archivo` | Nombre del archivo de salida |
| `-Wall` | Activar todos los avisos (warnings) importantes |
| `-DNOMBRE` | Definir la macro `NOMBRE` (equivale a `#define NOMBRE 1`) |
| `-DNOMBRE=valor` | Definir `NOMBRE` con un valor específico |
| `-g` | Incluir información de depuración (para usar con `gdb`) |
| `-O2` | Optimización nivel 2 |
| `-static` | Forzar enlazado estático |

## Referencia rápida: Herramientas de inspección

| Herramienta | Uso | Ejemplo |
|---|---|---|
| `wc -l` | Contar líneas de un archivo | `wc -l programa.i` |
| `grep` | Buscar patrón en archivo | `grep "printf" programa.s` |
| `head -N` | Ver primeras N líneas | `head -20 programa.s` |
| `nm` | Tabla de símbolos de `.o` o ejecutable | `nm programa.o` |
| `clang -Xclang -dump-tokens` | Análisis léxico (tokens) | ver sección Bonus |
| `clang -Xclang -ast-dump` | AST (análisis sintáctico+semántico) | ver sección Bonus |

---

## Ejercitación extra

Estos ejercicios son adicionales al laboratorio guiado. No se evalúan con los tests automáticos.

### E1 — Macros con efectos secundarios

La macro `CUADRADO(x)` definida como `((x) * (x))` tiene un problema sutil cuando el argumento tiene efectos secundarios. Analizá qué pasa con:

```c
int n = 3;
printf("%d\n", CUADRADO(n++));
```

a. ¿Cuántas veces se evalúa `n++`? ¿Por qué?
b. ¿Cuál sería el resultado? ¿Es el esperado?
c. ¿Cómo se resolvería este problema usando una función real en lugar de una macro?

### E2 — Provocar y leer errores del compilador

a. **Error léxico:** en `programa.c`, escribí una cadena sin cerrar: `printf("hola`. Intentá compilar con `gcc -S programa.c`. ¿En qué etapa falla y qué dice el mensaje de error?

b. **Error sintáctico:** quitá un `;` al final de una declaración de variable. ¿Qué reporta el compilador? ¿Menciona la línea correcta?

c. **Error semántico:** cambiá la llamada `sumar(3, 4)` por `sumar(3, 4, 5)`. ¿En qué etapa falla? ¿Por qué es semántico y no sintáctico?

d. **Error de enlazado:** comentá toda la implementación de `factorial` en `matematica.c` y recompilá solo el objeto (`gcc -c matematica.c -o matematica.o`). ¿Falla? Ahora intentá enlazar. ¿Cuándo falla y cuál es el mensaje exacto?

### E3 — Agregar una función nueva

a. Declarar en `matematica.h` el prototipo: `double potencia(double base, int exp);`
b. Implementar la función en `matematica.c` sin usar `<math.h>`.
c. Llamarla desde `main()` en `programa.c`.
d. Compilar paso a paso y verificar con `nm` que el símbolo `potencia` aparece como `U` en `programa.o` y como `T` en el ejecutable final.

### E4 — Enlazado estático vs dinámico

a. Compilar el programa con enlazado estático: `gcc programa.c matematica.c -o programa_static -static` (puede no funcionar en macOS; usar Linux o WSL).
b. Comparar el tamaño del ejecutable dinámico vs el estático con `ls -lh programa programa_static`.
c. Ejecutar `ldd programa_static` (Linux). ¿Qué diferencia hay respecto a `ldd programa`?

### E5 — Inspección con Clang

a. Ejecutar `clang -Xclang -dump-tokens programa.c 2>&1 | grep "programa.c" | wc -l`. ¿Cuántos tokens tiene `programa.c`?
b. Buscar en la salida del AST (`clang -Xclang -ast-dump`) la función `factorial`. ¿Cómo se representa la recursión en el árbol?
c. ¿Aparece algún `ImplicitCastExpr` en el AST? ¿Qué conversión realiza?
