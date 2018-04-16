# TP 1 - Fases de la Traducción y Errores
- Autor: Franco Giulianini Regueiro
- Legajo: 159.439-4
- Usuario GitHub: francoGiulianini
# Enunciado del TP:
4.1. Objetivos
• Identificar las fases de traducción y errores.
4.2. Temas
• Fases de traducción.
• Preprocesamiento.
• Compilación.
• Ensamblado.
• Vinculación (Link).
• Errores en cada fase.
4.3. Tareas
1. Investigar las funcionalidades y opciones que su compilador presenta para
limitar el inicio y fin de las fases de traducción.
2. Para la siguiente secuencia de pasos:
a. Transicribir en readme.md cada comando ejecutado y
b. Describir en readme.md el resultado u error obtenidos para cada paso.
4.3.1. Secuencia de Pasos
1. Escribir hello2.c, que es una variante de hello.c:
Secuencia de Pasos
#include <stdio.h>
int/*medio*/main(void){
int i=42;
prontf("La respuesta es %d\n");
2. Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su
contenido.
3. Escribir hello3.c, una nueva variante:
int printf(const char *s, ...);
int main(void){
int i=42;
prontf("La respuesta es %d\n");
4. Investigar la semántica de la primera línea.
5. Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias
entre hello3.c y hello3.i.
6. Compilar el resultado y generar hello3.s, no ensamblar.
7. Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar
hello4.s, no ensamblar.
8. Investigar hello4.s.
9. Ensamblar hello4.s en hello4.o, no vincular.
10. Vincular hello4.o con la biblioteca estándar y generar el ejecutable.
11. Corregir en hello5.c y generar el ejecutable.
12. Ejecutar y analizar el resultado.
13. Corregir en hello6.c y empezar de nuevo.
14. Escribir hello7.c, una nueva variante:
int main(void){
int i=42;
printf("La respuesta es %d\n", i);
}
Restricciones
15. Explicar porqué funciona.
4.4. Restricciones
• El programa ejemplo debe enviar por stdout la frase La respuesta es 42, el
valor 42 debe surgir de una variable.
# Hipótesis de Trabajo
La forma de abordar el trabajo se realizará siguiendo la lista de objetivos, haciendo hincapié en las normas de presentación, y resolviendo posibles inconvenientes buscando información de fuentes confiables para poder realizar el trabajo esperado de la manera esperada.
# Resultados obtenidos
- (Comando ejecutado: gcc hello2.c -E -P -o hello2.i) Al preprocesar hello2.c en hello2.i obtengo un texto muy distinto al original, ya que al usar el #include <stdio.h>, el preprocesador me "copy-pastea" todo el contenido del .h, que son muchísimos prototipos de funciones, alargando innecesariamente el texto, aunque no tenga un impacto en el rendimiento, ya que son sólo los prototipos de las funciones y no le dan más comportamiento al programa. Luego de la transcripción del .h, el programa fuente aparece, pero con los comentarios originales reeemplazados por espacios.
- (Comando ejecutado: gcc hello3.c -E -P -o hello3.i) Al preprocesar hello3.c, el .i obtenido es exactamente igual al original, ya que ahora que declaro el prototipo de la función que quiero usar de la biblioteca stdio.h, no es necesario copiarla toda. De todas maneras, la funcion prontf no es printf, pero no es trabajo del preprocesador chequear si la funcion esta o no definida, por lo tanto por ahora esto no sería un error.
- (Comando ejecutado: gcc hello3.i -S -P -o hello3.s) Al compilar hello3.i, el compilador me advierte de un error sintáctico: La función main tiene llaves no balanceadas. Debido a esto, no puedo compilar este archivo, y este sería un error del proceso de compilación.
- (Comando ejecutado: gcc hello4.c -S -P -o hello4.s) Al compilar hello4.c, esta vez sin el error anterior, el archivo generado es un texto en lenguaje ensamblador. Este texto sigue siendo entendible, aunque su nivel de abstracción claramente es más bajo que el .c original: este está manejando directamente los registros del sistema para lograr hacer lo que se le dijo inicialmente.
- (Comando ejecutado: gcc hello4.s -c -P -o hello4.o) Al ensamblar hello4.s, el texto resultante es ya una sucesión de números en hexadecimal. El texto original ya está en este punto convertido a código de máquina, el lenguaje que verdaderamente entiende el procesador. Lo que antes eran instrucciones que podian ser entendidas por un humano (aunque sean de bajo nivel), ahora son los números que representan a la misma operación, pero que el procesador si puede entender. Lo mismo ocurre con los registros sobre los cuales introducirá los datos.
- (Comando ejecutado: gcc hello4.o -P -o hello4.exe) Al querer transformar el .o en un ejecutable, y enlazarlo con la biblioteca standard para utilizar las funciones que invoca, el linker se da cuenta de que prontf no esta definida en la biblioteca standard, por lo que me devuelve un error de enlazado. Debo definir la función prontf para resolver este problema, o bien (como la intención original era usar printf), corregir el error renombrando prontf como printf.
- (Comando ejecutado: gcc hello5.o -P -o hello5.exe) Linkeo hello5.o y lo transformo en un ejecutable sin ningun problema, una vez corregido el error.
- Al ejecutar el programa, éste me devuelve que "La respuesta es 8282000". Claramente esto no era lo esperado, y esto se deba a un error en tiempo de ejecución: el compilador no puede darse cuenta de que le pase la cantidad "incorrecta" de parámetros a printf, porque técnicamente, es la cantidad correcta. El número se debe a que al querer darle formato de número decimal a un número que yo no le pase como parámetro, al ejecutarse el programa éste ejecuta la función, agarrando de algún lugar (tal vez al lado del string de formato) algo para darle formato decimal e imprimirlo por pantalla. En resumen, el comportamiento no puede ser definido si el error se da en tiempo de ejecución.
- Resuelvo el error en tiempo de ejecución con un hello6.c, y ahora el programa me devuelve "La respuesta es 42", tal como se quiso en un principio.
- Al ejecutar hello7.exe, este programa funciona, aunque no haya declarado en ningun lugar qué hace printf. En realidad, esto no es del todo cierto, ya que si bien ni siquiera se definió el prototipo de la función, al enlazar el archivo con la biblioteca standard (donde esta función efectivamente esta definida), el programa puede funcionar sin ningun problema, ya que en la última instancia del proceso de compilado el programa "descubre" que ahora si sabe que significa printf y qué hace, por lo que puede ser ejecutado sin ningún problema.