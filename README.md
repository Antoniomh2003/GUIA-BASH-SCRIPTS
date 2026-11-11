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
