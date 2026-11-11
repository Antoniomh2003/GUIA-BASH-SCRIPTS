# 📘 CHULETA DE BASH SCRIPTING (ASIR / ASO)
> **Formato ilustrativo:** Estructuras explicadas con ejemplos sencillos y reales (con `echo`, `date`, etc.) para que se vea exactamente qué poner en cada línea, sin ser un ejercicio de examen resuelto.

---

## 📌 ÍNDICE
1. [Cabecera, Permisos, Ejecución y Depuración](#1-cabecera-permisos-ejecución-y-depuración)
2. [Entrecomillados y Sustitución de Comandos](#2-entrecomillados-y-sustitución-de-comandos)
3. [Entrada y Salida Estándar (echo, read, IFS)](#3-entrada-y-salida-estándar-echo-read-ifs)
4. [Variables: Asignación, Especiales y de Entorno](#4-variables-asignación-especiales-y-de-entorno)
5. [Aritmética: Enteros, Decimales y Aleatorios](#5-aritmética-enteros-decimales-y-aleatorios)
6. [Metacaracteres y Utilidad basename](#6-metacaracteres-y-utilidad-basename)
7. [Comparadores en Corchetes [ ... ]](#7-comparadores-en-corchetes---)
8. [Estructuras Condicionales: IF y CASE con ejemplos claros](#8-estructuras-condicionales-if-y-case-con-ejemplos-claros)
9. [Bucles: FOR, WHILE y UNTIL con ejemplos claros](#9-bucles-for-while-y-until-con-ejemplos-claros)
10. [Menús Interactivos con SELECT](#10-menús-interactivos-con-select)
11. [Funciones y Variables Locales](#11-funciones-y-variables-locales)
12. [Salidas y Códigos de Retorno (exit vs return)](#12-salidas-y-códigos-de-retorno-exit-vs-return)
13. [Cajas de Diálogo: DIALOG con ejemplos claros](#13-cajas-de-diálogo-dialog-con-ejemplos-claros)
14. [Las 7 Trampas de Examen (Evita estos errores)](#14-las-7-trampas-de-examen-evita-estos-errores)

---

## 1. CABECERA, PERMISOS, EJECUCIÓN Y DEPURACIÓN

### Shebang obligatorio (1ª línea siempre)
```bash
#!/bin/bash
```

### Permisos y Ejecución en terminal
```bash
chmod +x script.sh          # Le da permiso de ejecución
chmod 700 script.sh         # Modo seguro: solo tu usuario puede leer, escribir y ejecutar
./script.sh                 # Ejecutarlo en la carpeta actual
sh script.sh                # Otra forma de ejecutarlo llamando a la shell
```
*Si quieres ejecutarlo como un comando del sistema (sin poner `./`):* se mueve a `/usr/bin/` con `chmod 777 script.sh`.

### Depuración paso a paso (Debug)
```bash
sh -x script.sh             # Muestra en pantalla cada orden ejecutada (+) con sus valores reales
sh -n script.sh             # Solo revisa si hay errores de sintaxis (no ejecuta nada)
export PS4='xtrace-> '      # Cambia el símbolo '+' por 'xtrace-> ' en el modo depuración
```

---

## 2. ENTRECOMILLADOS Y SUSTITUCIÓN DE COMANDOS

| Sintaxis | Nombre | Qué hace | Ejemplo real | Resultado |
| :---: | :--- | :--- | :--- | :--- |
| `' '` | **Comillas simples** | **Literal puro**. No interpreta `$var` ni comandos. | `echo 'Tu usuario es $USER'` | Muestra: `Tu usuario es $USER` |
| `" "` | **Comillas dobles** | **Expansión**. Sustituye variables y caracteres `\n`. | `echo "Tu usuario es $USER"` | Muestra: `Tu usuario es root` |
| `` ` ` `` o `$( )` | **Sustitución** | Ejecuta el comando y guarda/muestra su texto. | `FECHA=$(date +%d/%m/%Y)` | Guarda en la variable: `10/11/2026` |

---

## 3. ENTRADA Y SALIDA ESTÁNDAR (ECHO, READ, IFS)

```bash
# Mostrar texto con echo:
echo "Hola mundo"                             # Con salto de línea normal al final
echo -n "Introduce un número: "              # -n: se queda esperando en la misma línea
echo -e "Primera línea\nSegunda línea\tTab"   # -e: activa los saltos (\n) y tabuladores (\t)

# Leer datos por teclado con read:
read nombre                                   # Espera a que el usuario escriba y pulse Enter
read -p "Dime tu ciudad: " ciudad             # Muestra la pregunta antes de pedir el dato

# Cambiar el separador de campos (IFS):
# Si el usuario escribe "Madrid,Sevilla", guarda "Madrid" en CIUDAD1 y "Sevilla" en CIUDAD2:
IFS="," read -p "Introduce dos ciudades separadas por coma: " CIUDAD1 CIUDAD2
```

---

## 4. VARIABLES: ASIGNACIÓN, ESPECIALES Y DE ENTORNO

> ⚠️ **REGLA BÁSICA:** Al asignar variables **NUNCA debe haber espacios** junto al `=`.  
> `EDAD=20` (CORRECTO) | `EDAD = 20` (ERROR FATAL)

### Variables especiales automáticas
| Variable | Qué contiene | Ejemplo en la práctica |
| :---: | :--- | :--- |
| `$0` | Nombre del propio script | Si ejecutas `./menu.sh`, `$0` vale `./menu.sh` |
| `$1`, `$2` | Parámetros que le pasas al script | Si ejecutas `./script.sh pepe 25`, `$1` es `pepe` y `$2` es `25` |
| `$#` | Cantidad total de parámetros pasados | En `./script.sh pepe 25`, `$#` vale `2` |
| `$*` | Todos los parámetros como 1 sola frase | `"pepe 25"` |
| `$@` | Todos los parámetros separados uno a uno | `"pepe"` `"25"` |
| `$?` | Resultado del comando anterior | **0 = Todo ha ido bien (éxito)**, **Mayor que 0 = Hubo un error** |
| `$$` | PID del script en ejecución | Ej: `14250` (número de proceso) |
| `$RANDOM` | Número aleatorio entre 0 y 32767 | `ALEATORIO=$(( RANDOM % 10 + 1 ))` (da un número del 1 al 10) |

### Variables de entorno útiles
* `$USER` $\rightarrow$ Nombre de tu usuario actual (ej. `alumno` o `root`).
* `$HOME` $\rightarrow$ Tu carpeta personal (ej. `/home/alumno`).
* `$PWD` $\rightarrow$ Carpeta en la que te encuentras en este momento.

---

## 5. ARITMÉTICA: ENTEROS, DECIMALES Y ALEATORIOS

```bash
# 1. Forma moderna recomendada con $(( ... )):
A=10
B=3
SUMA=$(( A + B ))           # Resultado: 13
RESTA=$(( A - B ))          # Resultado: 7
MULTI=$(( A * B ))          # Resultado: 30
DIVISION=$(( A / B ))       # Resultado: 3 (división entera, sin decimales)
RESTO=$(( A % B ))          # Resultado: 1 (resto o módulo de dividir 10 entre 3)

# 2. Con let:
let RESULTADO=A+B
let CONTADOR++              # Suma 1 a la variable CONTADOR

# 3. Con comando expr (¡Ojo! El * se escapa obligatoriamente con \*):
RES=$(expr $A + $B)
RES=$(expr $A \* $B)        # Se pone \* porque si pones * solo, bash busca ficheros

# 4. Operaciones con decimales usando calculadora 'bc':
# scale=2 indica que queremos 2 decimales:
MEDIA=$(echo "scale=2; 10 / 3" | bc -l)      # Resultado: 3.33

# 5. Generar número aleatorio en un rango:
NUM=$(( (RANDOM % 10) + 1 ))                 # Genera un número del 1 al 10
```

---

## 6. METACARACTERES Y UTILIDAD BASENAME

### Metacaracteres de control
```bash
# ; -> Ejecuta una orden detrás de otra en la misma línea:
clear ; echo "Pantalla limpia"

# | (pipe) -> Envía la salida de un comando como entrada al siguiente:
cat /etc/passwd | grep bash

# && (AND) -> Solo se ejecuta la segunda orden si la primera tuvo ÉXITO ($? = 0):
mkdir prueba && cd prueba

# || (OR) -> Solo se ejecuta la segunda orden si la primera FALLÓ ($? != 0):
grep "juan" /etc/passwd || echo "El usuario juan no existe en el sistema"
```

### Utilidad `basename` (limpia rutas de ficheros)
```bash
# Quita la ruta y se queda solo con el nombre del archivo:
FICHERO=$(basename /var/log/syslog)       # Guarda: syslog

# Si además le pasas la extensión, también se la quita:
NOMBRE=$(basename /home/usuario/foto.png .png) # Guarda: foto
```

---

## 7. COMPARADORES EN CORCHETES `[ ... ]`

> ⚠️ **REGLA DE ORO:** Siempre deja un espacio después de `[` y antes de `]`.  
> Correcto: `if [ $A -gt $B ]` | Incorrecto: `if [$A -gt $B]`

### Comparar Números Enteros
| Operador | Significado | Ejemplo práctico |
| :---: | :--- | :--- |
| `-eq` | Igual a | `[ $edad -eq 18 ]` *(si edad es igual a 18)* |
| `-ne` | Distinto de | `[ $opcion -ne 0 ]` *(si opción es distinta de 0)* |
| `-gt` | Mayor que | `[ $intentos -gt 3 ]` *(si intentos es mayor que 3)* |
| `-ge` | Mayor o igual | `[ $nota -ge 5 ]` *(si nota es mayor o igual que 5)* |
| `-lt` | Menor que | `[ $precio -lt 50 ]` *(si precio es menor que 50)* |
| `-le` | Menor o igual | `[ $hora -le 12 ]` *(si hora es menor o igual que 12)* |

### Comparar Cadenas de Texto
| Operador | Significado | Ejemplo práctico |
| :---: | :--- | :--- |
| `"$A" = "$B"` | Son exactamente iguales | `[ "$respuesta" = "si" ]` |
| `"$A" != "$B"` | Son diferentes | `[ "$usuario" != "root" ]` |
| `-z "$A"` | La cadena está vacía | `[ -z "$nombre" ]` *(si no escribió nada)* |
| `-n "$A"` | La cadena NO está vacía | `[ -n "$nombre" ]` *(si sí tiene texto)* |

### Comprobar Ficheros y Carpetas
| Operador | Devuelve VERDADERO si... | Ejemplo práctico |
| :---: | :--- | :--- |
| `-e fichero` | El archivo o carpeta existe | `[ -e /etc/hosts ]` |
| `-f fichero` | Existe y es un fichero regular | `[ -f datos.txt ]` |
| `-d fichero` | Existe y es un directorio / carpeta | `[ -d /home/usuario ]` |
| `-s fichero` | Existe y NO está vacío (tamaño > 0) | `[ -s copia.log ]` |
| `-r fichero` | Tiene permiso de lectura | `[ -r informe.txt ]` |
| `-w fichero` | Tiene permiso de escritura | `[ -w salida.txt ]` |
| `-x fichero` | Tiene permiso de ejecución | `[ -x script.sh ]` |

---

## 8. ESTRUCTURAS CONDICIONALES: IF Y CASE (CON EJEMPLOS CLAROS)

### A. Estructura `if` / `elif` / `else` / `fi`
* **Qué hace:** Evalúa una condición; si se cumple hace una cosa, si se cumple otra opción hace otra, y si ninguna se cumple entra al `else` ("por defecto").

```bash
read -p "Introduce una nota del 1 al 10: " nota

if [ $nota -ge 9 ]; then
    echo "Sobresaliente"
elif [ $nota -ge 5 ]; then
    echo "Aprobado"
else
    # El 'else' es lo que se ejecuta por defecto si no entró ni en el if ni en el elif:
    echo "Suspenso"
fi
```
* **Explicación línea a línea:**
  * `if [ $nota -ge 9 ]; then` $\rightarrow$ Comprueba si `$nota` es mayor o igual que 9.
  * `elif [ $nota -ge 5 ]; then` $\rightarrow$ Si no fue $\ge 9$, comprueba si al menos es $\ge 5$.
  * `else` $\rightarrow$ Si no fue ni $\ge 9$ ni $\ge 5$, viene aquí automáticamente.
  * `fi` $\rightarrow$ Marca el final obligatorio del condicional.

---

### B. Estructura `case` / `esac`
* **Qué hace:** Compara una variable con varios patrones posibles. Es ideal para menús o respuestas de una sola letra.

```bash
read -p "Introduce una opción (a, b o s para salir): " letra

case $letra in
    a|A)
        echo "Elegiste la opción A"
        date                     # Muestra la fecha actual del sistema
        ;;
    b|B)
        echo "Elegiste la opción B"
        whoami                   # Muestra qué usuario tiene la sesión
        ;;
    s|S)
        echo "Saliendo del programa..."
        exit 0                   # Termina el script
        ;;
    *)
        # El '*' es el caso por defecto: se ejecuta si el usuario escribió cualquier otra cosa:
        echo "Opción no reconocida. Debes introducir a, b o s."
        ;;
esac
```
* **Explicación de las partes:**
  * `case $letra in` $\rightarrow$ Le decimos qué variable queremos comprobar (`$letra`).
  * `a|A)` $\rightarrow$ Si el valor es `a` o `A`. La barra `|` significa "o". El paréntesis `)` cierra el patrón.
  * `;;` $\rightarrow$ **¡Obligatorio al final de cada caso!** Indica a bash que ya terminó ese bloque.
  * `*)` $\rightarrow$ El caso comodín (por defecto): si el usuario escribió un número, una `z` o cualquier cosa no contemplada arriba.
  * `esac` $\rightarrow$ Cierra el `case` (la palabra `case` al revés).

---

## 9. BUCLES: FOR, WHILE Y UNTIL (CON EJEMPLOS CLAROS)

### A. Bucle `for` (Recorre elementos de una lista o secuencia)
```bash
# 1. Recorrer un rango del 1 al 5:
for numero in 1 2 3 4 5; do
    echo "Iteración número: $numero"
done

# 2. Recorrer un rango automático con llaves (del 1 al 10):
for i in {1..10}; do
    echo "Contando: $i"
done

# 3. Recorrer una secuencia con salto (inicio, incremento, fin):
# En este ejemplo cuenta de 2 en 2 del 1 al 9: 1, 3, 5, 7, 9
for valor in $(seq 1 2 9); do
    echo "Valor: $valor"
done

# 4. Recorrer todos los ficheros .txt de la carpeta actual:
for archivo in *.txt; do
    echo "He encontrado el archivo: $archivo"
done

# 5. Recorrer todos los parámetros pasados al script desde la terminal:
for parametro in "$@"; do
    echo "Parámetro recibido: $parametro"
done

# 6. Bucle estilo C clásico (contador que empieza en 0 hasta 4):
for (( i=0; i<5; i++ )); do
    echo "Índice C: $i"
done
```

---

### B. Bucle `while` (Se repite mientras la condición sea VERDADERA)
```bash
contador=1

while [ $contador -le 5 ]; do
    echo "El contador vale: $contador"
    contador=$(( contador + 1 ))   # Sumamos 1 en cada vuelta para que no sea infinito
done
```

#### Cómo leer un archivo de texto línea a línea con `while`:
```bash
# Lee el archivo /etc/hosts línea por línea y la muestra por pantalla:
while read linea; do
    echo "Línea leída: $linea"
done < /etc/hosts
```

---

### C. Bucle `until` (Se repite hasta que la condición sea VERDADERA / mientras sea FALSA)
*A diferencia de `while`, el bucle `until` se sigue repitiendo mientras la condición NO se cumpla:*
```bash
palabra=""

until [ "$palabra" = "secreto" ]; do
    read -p "Introduce la palabra clave para continuar: " palabra
done

echo "¡Acceso concedido!"
```

---

### D. Romper o saltar vueltas en bucles: `break` y `continue`
* `break` $\rightarrow$ Detiene el bucle por completo y se sale de él.
* `continue` $\rightarrow$ Salta lo que quede de esa vuelta y pasa directamente a la siguiente iteración.

---

## 10. MENÚS INTERACTIVOS CON `select`

*El comando `select` crea automáticamente una lista numerada en la terminal y se queda esperando que el usuario teclee el número correspondiente:*

```bash
# PS3 es la frase que sale como pregunta para elegir opción:
PS3="Elige una opción del menú: "

select opcion in "Ver fecha" "Ver usuarios conectados" "Salir"; do
    case $opcion in
        "Ver fecha")
            date
            ;;
        "Ver usuarios conectados")
            who
            ;;
        "Salir")
            echo "Hasta luego"
            break                      # El break rompe el menú select y sale
            ;;
        *)
            # Si el usuario teclea 8 o letras, $REPLY contiene lo que tecleó:
            echo "Opción no válida. Tecleaste: $REPLY"
            ;;
    esac
done
```

---

## 11. FUNCIONES Y VARIABLES LOCALES

*Una función es un bloque de código reutilizable con un nombre:*

```bash
# 1. Definición de la función:
saludar_usuario() {
    # NOTA: $1 dentro de la función es el primer argumento que le pases A LA FUNCIÓN
    local nombre_persona=$1           # 'local' asegura que no modifique variables de fuera
    echo "Hola, bienvenido al script $nombre_persona"
    return 0                          # Devuelve código 0 (éxito) al script
}

# 2. Llamada a la función (¡se escribe SIN paréntesis!):
saludar_usuario "Antonio"
saludar_usuario "Carlos"
```

---

## 12. SALIDAS Y CÓDIGOS DE RETORNO (`exit` vs `return`)

* **`exit` $\rightarrow$ Apaga y cierra el script por completo:**
  * `exit 0` $\rightarrow$ Termina indicando que todo funcionó correctamente.
  * `exit 1` $\rightarrow$ Termina indicando que ocurrió un error.
* **`return` $\rightarrow$ Finaliza únicamente una función:**
  * No detiene el script completo, solo sale de esa función y deja el resultado disponible en la variable `$?`.

---

## 13. CAJAS DE DIÁLOGO: `dialog` (CON EJEMPLOS CLAROS)

*El comando `dialog` dibuja ventanas gráficas en la terminal.  
**Detalle clave:** como la ventana se dibuja en la pantalla, las respuestas escritas por el usuario deben guardarse con `2>fichero_temporal` para luego leerlas.*

### A. Caja de aviso simple (`--msgbox` y `--infobox`)
```bash
# Muestra un mensaje y un botón de "Aceptar":
dialog --msgbox "Operación completada con éxito." 8 40

# Muestra un mensaje breve y desaparece de inmediato:
dialog --infobox "Cargando datos del sistema..." 5 35
```

### B. Pregunta de confirmación Sí / No (`--yesno`)
*Si el usuario elige **Sí**, devuelve un **0**. Si elige **No**, devuelve un **1**:*
```bash
dialog --yesno "¿Deseas borrar la carpeta temporal?" 8 40

if [ $? -eq 0 ]; then
    echo "El usuario pulsó SÍ"
else
    echo "El usuario pulsó NO"
fi
```

### C. Pedir un texto por pantalla (`--inputbox`)
```bash
# Pide un dato y guarda lo que escribe el usuario en /tmp/dato.txt:
dialog --inputbox "Introduce tu nombre:" 10 35 2>/tmp/dato.txt

# Leemos la respuesta que se guardó en el archivo:
NOMBRE=$(cat /tmp/dato.txt)
rm -f /tmp/dato.txt                   # Borramos el temporal

echo "El nombre introducido fue: $NOMBRE"
```

### D. Crear un menú con opciones (`--menu`)
```bash
# Formato: dialog --menu "Título" <alto> <ancho> <nº_opciones> tag1 "desc1" tag2 "desc2" 2>tmp
dialog --menu "Menú de Mantenimiento" 15 40 3 \
    1 "Ver espacio en disco" \
    2 "Ver usuarios del sistema" \
    3 "Salir" 2>/tmp/opcion_elegida.txt

ELECCION=$(cat /tmp/opcion_elegida.txt)
rm -f /tmp/opcion_elegida.txt

case $ELECCION in
    1) df -h ;;
    2) cat /etc/passwd ;;
    3) exit 0 ;;
esac
```

---

## 14. LAS 7 TRAMPAS DE EXAMEN (EVITA ESTOS ERRORES)

1. 🚫 **Espacios en asignaciones:**
   * ❌ `MI_VAR = 10` $\rightarrow$ Bash cree que `MI_VAR` es un programa y da *"orden no encontrada"*.
   * ✅ `MI_VAR=10` $\rightarrow$ Siempre todo pegado.
2. 🚫 **Espacios dentro de los corchetes:**
   * ❌ `if [$A -eq 5]` o `if [ $A -eq 5]` $\rightarrow$ Da error sintáctico.
   * ✅ `if [ $A -eq 5 ]` $\rightarrow$ Obligatorio dejar espacio tras `[` y antes de `]`.
3. 🚫 **Comparar números con signos de texto:**
   * ❌ `if [ $nota == 10 ]` $\rightarrow$ Puede fallar o dar comportamientos extraños.
   * ✅ `if [ $nota -eq 10 ]` $\rightarrow$ Para enteros usa siempre `-eq`, `-ne`, `-lt`, `-gt`.
4. 🚫 **Olvidar el `;;` en cada rama de `case`:**
   * Cada bloque de un `case` DEBE terminar obligatoriamente con dos puntos y coma: `;;`.
5. 🚫 **No cerrar las estructuras con su palabra al revés:**
   * `if` se cierra con `fi`.
   * `case` se cierra con `esac`.
   * `for`, `while` y `until` se cierran con `done`.
6. 🚫 **El asterisco en `expr`:**
   * ❌ `expr 4 * 5` $\rightarrow$ Falla porque `*` se interpreta como comodín de archivos.
   * ✅ `expr 4 \* 5` o mucho más fácil: `$(( 4 * 5 ))`.
7. 🚫 **Parámetros en funciones:**
   * Dentro de una función, `$1` es el argumento pasado **a la función**, no al script general.
