# LDmicro  

## Implementación en OSIMPLC  



> Esta traducción del manual de LDmicro al español ha sido editada específicamente para su utilización con OSIMPLC, en base al contenido del archivo manual.txt en inglés provisto con la descarga de la versión v.5.4.0.4 de LDmicro.  
> Esta edición pretende ampliar, corregir y mejorar la traducción original del manual en español, provista como manual-es.txt en dicha versión del programa.
> 
> Al editar la presente versión, se ha modificado la organización en la presentación de los diferentes ítems, en particular la referida a las diferentes instrucciones Ladder implementadas por LDmicro.  
> El objetivo de esta reorganización ha sido presentar las diferentes instrucciones agrupándolas en función del tipo de proceso o actividad que ejecutan, y por la complejidad en su implementación.  
> También se han simplificado algunos de los gráficos complementarios incluídos en modo texto, para permitir su completa edición en formato Markdown (.md), y para facilitar la generación de código HTML para su presentación en la web sin requerir hipervínculos a archivos de imágenes externos.

---

## INTRODUCCION

LDmicro genera código nativo para ciertos Microcontroladores Microchip PIC16 y Atmel AVR.  
Por lo general, el software para estos microcontroladores está escrito en lenguaje de programación como ensamblador, C o BASIC.  
Un programa en uno de estos lenguajes comprende una lista de enunciados.  
Estos lenguajes son potentes y bien  adaptados a la arquitectura del procesador, que ejecuta internamente una lista de instrucciones.  

Los PLCs, por otro lado, se programan a menudo en Ladder, lenguaje "escalera" o "de contactos".  
Un programa simple desarrollado en Ladder podría verse así:

```
   ||                                                                    ||
   ||    Xbutton1           Tdon           Rchatter           Yred       ||
   ||-------]/[---------[TON 1.000 s]-+-------]/[--------------( )-------||
   ||                                 |                                  ||
   ||    Xbutton2           Tdof      |                                  ||
   ||-------]/[---------[TOF 2.000 s]-+                                  ||
   ||                                                                    ||
   ||                                                                    ||
   ||                                                                    ||
   ||    Rchatter            Ton             Tnew           Rchatter     ||
   ||-------]/[---------[TON 1.000 s]----[TOF 1.000 s]---------( )-------||
   ||                                                                    ||
   ||                                                                    ||
   ||                                                                    ||
   ||------[END]---------------------------------------------------------||
   ||                                                                    ||
   ||                                                                    ||
```

En el ejemplo, TON es un retardo a la activación, TOF es un retardo a la desactivación, los símbolos `--]`, `[--` y `--]/[--` son entradas de señal lógica que se comportan como los contactos de un relé, los símbolos `--( )--` son salidas de señal lógica que se comportan como la bobina de un relé.

Existen buenas referencias para la lógica de contactos, disponibles libremente en Internet.  

Los detalles específicos de esta implementación en LDmicro se explican a continuación.  

Son evidentes las siguientes diferencias:  

* El programa se presenta en formato gráfico, no como una lista textual de instrucciones.  
Muchas personas inicialmente encontrarán ésto más fácil de entender.  

* En el nivel más básico, los programas parecen diagramas de circuito, con contactos de relé (entradas) y bobinas (salidas).  
Esto resulta más intuitivo para los programadores con conocimiento de teoría de circuitos eléctricos.  

* El compilador lógico del lenguaje de contactos se encarga de cuándo, dónde y cómo se deben recalcular los estados del sistema.  

No es necesario escribir código para determinar cuándo las salidas deben actualizarse p. ej. sobre la base de un cambio en las entradas o en un temporizador, y no es necesario especificar el orden en que estos cálculos deben tener lugar. Las herramientas del PLC hacen eso por usted.  

LDmicro compila lógica de contactos para PIC16 o código AVR. Los siguientes microcontroladores son soportados:  

* PIC16F628
* PIC16F628A
* PIC16F876
* PIC16F877A
* PIC16F88
* PIC16F819
* PIC16F886
* PIC16F887
* ATmega8
* ATmega16
* ATmega32
* ATmega64
* ATmega128
* ATmega162
* ATmega1284P
* ATmega2560  

 Verifique la lista de microcontroladores soportados mediante el menú Configuraciones > Microcontrolador. La lista para versiones >= v.5.3.0 es mucho más extensa, e incluye algunos microcontroladores AVR AT90 y ARM STM32.  
> Consulte en el foro de LDmicro el estado de soporte para las diversas líneas y modelos de microcontroladores. Es habitual que se incorporen más modelos al liberar una nueva versión.  

Usando LDmicro, puede diseñar un diagrama de contactos para su programa. Usted también puede simular la ejecución en tiempo real de la lógica en su PC. Una vez que esté convencido que el diagrama es correcto, puede asignar pines en el microcontrolador a la entradas y salidas del programa.  
Una vez que haya asignado los pines, puede compilar el código PIC o AVR para su programa. La salida del compilador es un fichero hexadecimal *.hex,  archivo que puede descargar en su microcontrolador utilizando cualquier programador PIC / AVR.  

LDmicro está diseñado para ser similar a la mayoría de los sistemas de programación de PLC comerciales. Hay algunas excepciones, y muchas cosas no son estándar en la industria de todos modos.  
Lea atentamente la descripción de cada instrucción, aunque le parezca familiar. Este documento presupone el conocimiento básico de la lógica de contactos y de la estructura del software del PLC (ciclo de ejecución: lectura de entradas, cálculo, escritura de salidas).  

Ver [LDmicro Wiki - PLC and safety](https://github.com/LDmicro/LDmicro/wiki/PLC-and-safety) y [LDmicro Wiki - PLC Cycle Time 0](https://github.com/LDmicro/LDmicro/wiki/PLC-Cycle-Time-0)  

---

## OBJETIVOS ADICIONALES

Con LDmicro también es posible generar código ANSI C. Usted podría usar ésto con cualquier procesador para el que se disponga de un compilador C, pero usted es responsable de suministrar el ciclo de ejecución (runtime).  

Ésto significa que LDmicro sólo genera fuentes para la función PlcCycle(). Usted es responsable de llamar a PlcCycle en cada ciclo, y usted es responsable de implementar todas las E/S (lectura de Entradas/ escritura, etc.) que la función PlcCycle() llama. Vea los comentarios en la fuente generada para obtener más detalles.  

Por último, LDmicro puede generar bytecode independiente del procesador para un máquina virtual diseñada para ejecutar código de lógica de contactos.  

He proporcionado un ejemplo de implementación del intérprete / VM, escrito en C estandard. Esta meta funcionará para casi cualquier plataforma, siempre y cuando usted pueda suministrar su propia VM. Esto podría ser útil para aplicaciones en las que desea usar la lógica ladder como un "lenguaje de scripting" para personalizar un programa. Vea los comentarios en el ejemplo de intérprete para más detalles.  

Se ha añadido un nuevo objetivo "Controllino Maxi / Ext bytecode". Genera un archivo .xint interpretable por el software de PLC LDuino. Hasta ahora sólo el controlador Maxi PLC es soportado. Sin embargo, como el bytecode es genérico, se podría hacer la adaptación a cualquier otro PLC o tarjeta de la CPU. Vea el código fuente de LDuino para eso.  

---

## OPCIONES DESDE LINEA DE COMANDOS

`ldmicro.exe` habitualmente se ejecuta desde la línea de comandos sin opciones.  

Ésto significa que basta con hacer un acceso directo al programa o guardarlo en su escritorio, y hacer doble click en el ícono cuando desee ejecutarlo; a continuación, puede realizar toda la tarea de programación, asignación y compilación dentro del Entorno Gráfico de Usuario de LDmicro.  

Si en la línea de comandos se pasa a LDmicro un nombre de archivo único (por ejemplo, 'ldmicro.exe asd.ld'), entonces LDmicro intentará abrir el archivo "asd.ld", si existe. Se produce un error si "asd.ld" no existe.  
Ésto significa que se puede asociar ldmicro.exe con archivos .ld, para que se ejecute automáticamente al hacer doble click en un archivo .ld.  

Si se pasa argumentos de línea de comandos a LDmicro en la forma `Ldmicro.exe / c src.ld dest.hex`, entonces el ejecutable intenta compilar `src.ld`, y guarda la salida como `dest.hex` .  

LDmicro finaliza después de compilar, tanto si la compilación resultó exitosa o no. Los mensajes se imprimen en la consola. Este modo sólo es útil cuando se ejecuta LDmicro desde línea de comandos.  

Si se ejecuta LDmicro sin argumentos, se comienza con un archivo de programa vacío. Si se ejecuta LDmicro con el nombre de un programa de lenguaje de contactos (xxx.ld) en la línea de comandos, entonces se intentará cargar ese programa en el inicio.  

---

## CONCEPTOS BASICOS

LDmicro utiliza su propio formato interno para el programa; no puede importar lógica de cualquier otra herramienta o programa equivalente.  

Si no se cargó un programa ya existente, al ejecutar LDmicro se iniciará un programa con un diagrama de contactos vacío.  
Puede modificar el programa insertando o eliminando instrucciones.  

El cursor en la pantalla del programa parpadea para indicar la instrucción y el punto de inserción actual. Si no está parpadeando entonces pulse la tecla Tab o haga clic en una instrucción. Ahora puede borrar o puede insertar una nueva instrucción a la derecha o a la izquierda (en serie con), o por encima o por debajo (en paralelo con), la instrucción seleccionada.  

Algunas operaciones no están permitidas, por ejemplo, no hay instrucciones a la derecha de una bobina de salida.  

El programa comienza con una sola línea (escalón, rung). Puede agregar más escalones seleccionando *Insertar Línea Antes | Después* en el menú Editar.  

Podría obtener el mismo resultado lógico colocando muchos subcircuitos complicados en paralelo en un línea, pero es más claro utilizar múltiples líneas (escalones).  

Usted puede agregar instrucciones; por ejemplo puede agregar un conjunto de contactos (menú Instrucción -> Insertar contactos) denominados "Xnew".  

"X" en este caso significa que el contacto estará vinculado a un pin de entrada en el Microcontrolador. Puede asignarle el pin correspondiente más adelante, después de seleccionar el modelo del microcontrolador y cambiar el nombre de los contactos.  

La primera letra de un nombre (variable) indica qué tipo de objeto es.  

### Variables (objetos) actuales en LDmicro

* Xname - X: pin de entrada digital en el microcontrolador  
* Yname - Y: pin de salida digital en el microcontrolador  
* Rname - R: relay interno o marca, un bit en la memoria del microcontrolador  
* Tname - T: temporizado a la conexión, a la desconexión, acumulativo, etc.  
* Cname - C: contador incremental, decremental, circular, etc.  
* Aname - A: variable de un entero leída desde un convertidor A/D  
* Pname - P: salida PWM en el microcontrolador  
* Mname - M: bobina discreta MODBUS  
* Iname - I: entrada discreta MODBUS  
* Hname - H: registro de variable MODBUS  
* Name - nombre: variable de uso general (un entero con signo). Tipos actualmente soportados: byte, word, word+nibble, dword; 1, 2, 3 y 4 bytes.  

**IMPORTANTE:**

* Los nombres de las variables pueden consistir en letras, números y subrayados "_".  
* El nombre de una variable no debe comenzar con un número.  
* En los nombres de variables se distinguen mayúsculas y minúsculas (case sensitive).  
* No utilice las letras mayúsculas X, Y, R, T, C, A, P, M, I, H, como letra inicial de los nombres de variables de uso general.  

Elija el resto del nombre de la variable para que describa convenientemente lo que hace el objeto, y de modo que sea único dentro del programa.  
El mismo nombre siempre se refiere al mismo objeto dentro del programa.  

Por ejemplo, sería un error tener un retardo a la conexión (TON) llamado "Tespera" y un retardo a la desconexión (TOF) llamado también "Tespera" en el mismo programa, ya que cada temporizador necesita su propio espacio de memoria.  
Por otro lado, sería correcto tener un temporizador retentivo (RTO) llamado "Tsumar" y una instrucción de restablecimiento (RES) asociada con "Tsumar", ya que en este caso se desea que ambas instrucciones trabajen con el mismo temporizador.  

### Direccionamiento directo e indirecto a los registros del microcontrolador   

- Un nombre de variable que comienza con el símbolo '#' como  #PORTA, #PORTB, #PORTC, ... es tratado como puerto de salida del hardware.  
- Un nombre de variable que comienza con el símbolo '#' como #PINA, #PINB, #PINC, ... es tratado como puerto de entrada del hardware.  
- Un nombre de variable que comienza con el símbolo '#' como #TRISA, #TRISB, #TRISC, ...  es tratado como registro de dirección de datos de puertos correspondientes #PORTA, #PORTB, #PORTC, ...  
- Un nombre de variable que comienza con un carácter '#' y un número subsecuente (comúnmente un hexadecimal) es tratado como la dirección explícita (directa) del correspondiente registro en el hardware.  
- Un nombre de variable que comienza con un carácter '#' y un nombre de variable de uso general es tratado como un puntero (direccionamiento indirecto a un registro del hardware). Variables como  #VarName son la dirección indirecta de un registro (Indirect Address Pointer).  
  
> Por ejemplo: 

```
{MOV  portAddr:=}  ; portAddr es una variable de uso general a la que se le asigna el valor 0x05.  
--{0x05}--  
{MOV #portAddr:=}   ; portAddr es tratada como el puntero indirecto al registro #0x05 PORTA.  
--{0xF0}--  ; los pines en PORTA (salidas) son escritos con el valor binario 11110000.  
```

¡Tenga cuidado al escribir en los registros del hardware mediante el acceso directo a sus direcciones!  

Ver [LDmicro Wiki - Indirect addressing](https://github.com/LDmicro/LDmicro/wiki/LDmicro-indirect-addressing)  

Las instrucciones disponibles para variables generales (MOV, ADD, EQU, etc.) pueden ejecutarse sobre variables con cualquier nombre, inclusive las que comienzan con T (temporizadores), C (contadores).  

Ésto significa que pueden acceder a objetos como temporizadores y a contadores para realizar operaciones (comparaciones, cálculos aritméticos, etc.) con los valores actuales de sus variables.  

A veces ésto puede ser muy útil; por ejemplo, usted podría comprobar si el conteo de un temporizador está dentro de un rango determinado.

Las variables siempre se tratan como enteros con signo. Usted puede especificar literales como números decimales normales (10, 1234, -56). También puede especificar códigos de carácter ASCII ('A', 'z', '5', etc.) colocando el carácter en comillas simples. Puede utilizar un código ASCII en la mayoría de los lugares en que puede utilizar un número decimal.

Puede también utilizar números hexadecimales (0xA, 0x04D2, 0xffc8), números octales (0o12, 0o2322, 0o177710), o números binarios (0b1010, 0b10011010010, 0b1111111111001000), en la mayoría de los lugares en los que podría utilizar un número decimal.  

LDmicro utiliza los prefijos del lenguaje C:  

+ 0x__ ó 0X__ para números hexadecimales con dígitos 0123456789ABCDEF  
+ 0o__ ó 0O__ o 0__ números octales con dígitos 01234567   
+ 0b__ ó 0B__ para números binarios con dígitos 01  

> Nota: la notación binaria y la hexadecimal resultan más adecuadas para las operaciones bit a bit.


#### Actualización: Versiones >= v.4.3.0

Las variables pueden alojar números enteros con signo de 1, 2, 3 ó 4 bytes.  

Versiones anteriores sólo trabajan con variables de 2 bytes, con rango -32768 ~ +32767.

Se puede cambiar el tamaño de la variable (alcance, span) mediante doble click en su nombre en la lista de variables en la zona inferior de la ventana de LDmicro.  

Las variables son almacenadas y procesadas en la forma de complemento a 2.  

Ver [Two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)  

Alcance de las variables:

```
Bytes|   Tipo       |        Rango desde               |            hasta                 |
   1 | signed int8  | -2^7  = -128 = 0x80              | 2^7 -1 = 127 = 0x7F              |
   2 | signed int16 | -2^15 = -32768 = 0x8000          | 2^15-1 = 32767 = 0x7FFF          |
   3 | signed int24 | -2^23 = -8388608 = 0x800000      | 2^23-1 = 8388607 = 0x7FFFFF      |
   4 | signed int32 | -2^31 = -2147483648 = 0x80000000 | 2^31-1 = 2147483647 = 0x7FFFFFFF |
```

El valor decimal cero (0) es representado por los bits 00...000  

El valor decimal menos uno (-1) es representado por los bits 11...111  

Las variables con signo int8 (byte) pueden ser utilizadas para que el archivo .hex resulte más pequeño y rápido de ejecutar.  
Las variables con signo int16 (entero, integer, word) pueden ser utilizadas habitualmente para operaciones aritméticas y con los valores obtenidos de la lectura de los ADC, y son compatibles con versiones de LDmicro previas a v.4.3.0.  
Las variables con signo int24 y int32 (dword) pueden ser utilizadas para ampliar el rango de la operación, a costa de incrementar considerablemente el tamaño del archivo .hex y hacer más lenta la ejecución del código máquina en el microcontrolador.  
La extensión del signo para variables de diferentes tamaños es proporcionada automáticamente en LDmicro.  

> Nota: las instrucciones MUL, DIV, MOD, no pueden procesar variables int32 con signo.  

### Marca de desbordamiento

La marca de desbordamiento (Overflow|Underflow ó Carry/Borrow) es provista como el relé interno ROverflowFlagV en LDmicro.  

Ver [Overflow_flag](https://en.wikipedia.org/wiki/Overflow_flag) y [Binary-overflow](https://www.allaboutcircuits.com/textbook/digital/chpt-2/binary-overflow/)  

La marca de desbordamiento ROverflowFlagV indica que el resultado en complemento a dos de una operación con signo (aritmética, asignación, etc.) no cabe en el número de bits utilizado en la variable destino de la operación, y señala un error que debe ser resuelto por el usuario.  

Por ejemplo, si a una variable del tipo int8 (byte) dest = 127 (0x7f) se le agrega 1, se obtiene -128 (0x80), y la marca de desbordamiento ROverflowFlagV será establecida en 1.  
Por el contrario, si a una variable int16 (word) dest = 127 (0x007f) se le agrega 1, se obtiene 128 (0x0080), y la marca de desbordamiento ROverflowFlagV no será afectada.  

LDmicro repone a 0 la marca de desbordamiento ROverflowFlagV durante la inicialización (Power On o Reset por software o hardware).  

> Nota: CTC genera un impulso de sobrellenado (Overfill Carry) cuando Counter==Max.  
>      CTR genera un impulso de sobrellenado (Overfill Borrow) cuando Counter==Min.  
>      Overfill (Carry|Borrow) no establece a 1 la marca de desbordamiento ROverflowFlagV.  

### Marca de cambio de signo

La marca de cambio de signo (traslapo) es provista como el estado de salida de las operaciones suma (ADD) y resta (SUB).  

La marca de superposición indica que ha cambiado el signo en el resultado de la operación.  

Por ejemplo, ocurre traslapo cuando a -1 (0xf..f) se le suma 1 (o se le resta -1),se obtendrá 0 (0x0..0), todos los dígitos en uno (1 )en la variable cambian a cero (0).  También ocurre traslapo cuando a 0 (0x0..0) se le resta 1 (o se le suma -1), se obtendrá 1 (0xF..F), todos los dígitos en cero (0) cambian a uno (1).  

Otro ejemplo: ocurre traslapo cuando a -10 se le suma 15 (o se le resta -15), se obtendrá 5; la variable origen negativa cambiará a un resultado positivo. También ocurre traslapo cuando a 10 se le resta 15 (o se le suma -15), se obtendrá -5; la variable origen positiva cambiará a un resultado negativo.  

---

## SIMULACION

Una vez que haya escrito un programa, puede probarlo en simulación, y luego puede compilarlo en un archivo .hex para el microcontrolador de destino.  

Para entrar en el modo de simulación, seleccione Simular -> Modo de simulación o pulse las teclas Ctrl+M o F7.  
El programa se muestra de forma diferente en el modo de simulación. Ya no está visible el cursor.  
Las instrucciones que están activadas aparecen en rojo, las instrucciones que están desactivadas aparecen en gris.  
Presione la barra espaciadora para ejecutar un ciclo del PLC.  
Para realizar un ciclo continuo en tiempo real, seleccione Simular -> Iniciar Simulación en tiempo real, o presione las teclas Ctrl+R ó F8. La visualización del programa se actualizará en tiempo real, a medida que cambien los estados de contactos, bobinas, temporizadores, contadores u otros objetos presentes en el programa.  

Puede configurar el estado de las entradas de señal en el programa (pines de entrada en el microcontrolador) haciendo doble click en la lista en la parte inferior de la ventana, o haciendo doble click en el nombre del contacto correspondiente "Xname" en el programa en Ladder.  
Si desea que una entrada adopte el valor 1 (ON, True) automáticamente al inicio de la simulación, preceda dicha entrada con el símbolo ^.  
Si cambia el estado de un pin de entrada, ese cambio no se reflejará en la visualización del programa hasta que el PLC ejecute un nuevo ciclo.  
Ésto sucedera automáticamente si está ejecutando una simulación en tiempo real, o cuando presione la barra espaciadora si está simulando la ejecución ciclo por ciclo.  

---

## COMPILANDO A CODIGO MAQUINA

En última instancia, el objetivo de la programación bajo LDmicro es generar el código máquina que se pueda descargar en la memoria del microcontrolador para ser ejecutado por el mismo.  

Para ello, se debe generar el archivo .hex (formato Intel IHEX) mediante la compilación del esquema Ladder. El archivo .hex contendrá código intermedio que permitirá grabar las distintas posiciones de las memorias del microcontrolador (Flash, programa; EEPROM, datos; Config Bits, configuración) utilizando un programador ("quemador") con hardware y software específicos.  

### SELECCION DEL MICROCONTROLADOR

En primer término, deberá seleccionar el modelo del microcontrolador en el menú  Configuración -> Microcontrolador.  

Tenga muy en consideración que algunos modelos de microcontroladores actualmente soportados por LDmicro son comercializados en diferentes formatos (DIP, QFP, etc.).  

En caso de utilizar un microcontrolador compatible en algún formato diferente al presentado en la lista, asigne las entradas y salidas en el programa de usuario a los pines correspondientes de acuerdo a su función, Puerto y número dentro del mismo; no las asigne según el número de pin en la huella (footprint) del circuito integrado.  

### CONFIGURACIÓN DE PARÁMETROS DEL MICROCONTROLADOR  

A continuación deberá configurar los parámetros de trabajo del microcontrolador en el menú Ajustes -> Parámetros MCU ....  

Deberá configurar el tiempo de ciclo con el que va a ejecutar el programa (ciclo de PLC), y deberá indicarle al compilador con qué frecuencia base funcionará el microcontrolador (frecuencia del cristal externo u oscilador).  

En general, usted no necesitará cambiar el tiempo del ciclo preestablecido; 10 ms es un buen valor para la mayoría de las aplicaciones. Sin embargo, para tareas que requieren alta velocidad en el procesamiento de entradas/salidas, tiempo de ciclos menores pueden resultar provechosos. Por otra parte, si el programa contiene una muy gran cantidad de instrucciones, tiempos de ciclo mayores pueden permitir la ejecución sin que resulte truncada por el temporizador de control WDT.  

Escriba la frecuencia del cristal (o el resonador cerámico, etc.) que va a usar con el microcontrolador. Ésta frecuencia base será la utilizada para que el compilador calcule las rutinas de temporizadores y otras rutinas internas.  

Si se han programado instrucciones que operen comunicación de datos por UART, también deberá definir la velocidad de transmisión y recepción (baudrate).  

En las versiones actuales de LDmicro, también podrá establecer los bits de configuración para microcontroladores de la línea PIC (aceptando ó modificando el valor por defecto generado por LDmicro), y los bits de configuración de los microcontroladores de la línea AVR .  

Además, deberá establecer la frecuencia de los buses SPI e I2C, en caso de utilizar las correspondientes instrucciones en el programa de usuario.  

### ASIGNACION DE PINES A ENTRADAS Y SALIDAS DIGITALES, Y A ENTRADAS ANALOGICAS  

En la parte inferior de la ventana verá una lista conteniendo todos los objetos del programa (contactos, bobinas, temporizadores, contadores, variables generales, etc. etc.).  
Esta lista se genera automáticamente desde el programa, no hay necesidad de mantenerla actualizada manualmente.  

La mayoría de los objetos no necesita de ninguna configuración. Sin embargo, los objetos "Xname" (entrada digital), "Yname" (salida digital) y "Aname" (variable de lectura de ADC) deben ser obligatoriamente asignados a un pin en el microcontrolador.  

Primero deberá elegir en el menú Configuración -> Microcontrolador qué modelo de microcontrolador será utilizado, y a continuación deberá asignar los pines de E/S haciendo doble click en los objetos en la lista.  

Luego deberá asignar un pin de E/S a cada objeto "Xname", "Yname" y "Aname".  
Puede hacerlo mediante doble click en el nombre del objeto en la lista en la parte inferior de la ventana. Aparecerá un cuadro de diálogo mostrando una lista con la identificación de cada uno de los pines y sus posibles funciones, en la que podrá elegir un pin aún no asignado.  

Los pines destinados a comunicación UART y PWM son asignados automáticamente por LDmicro en función del microcontrolador seleccionado.  

Si desea remover la asignación de un objeto a un determinado pin para reasignarlo a otro, selecciónelo y asígnele el valor "(no pin)" que se muestra en la primer línea en la lista, luego selecciónelo nuevamente y asígnele el nuevo pin deseado.  

> **NOTA: PULL UP RESISTORS**  
> Tenga muy en consideración, al diseñar su hardware y/o editar el programa de usuario, todas las configuraciones de resistencias internas (pull up resistors) que LDmicro establece por defecto en los pines asigados a entradas digitales en algunos puertos de los microcontroladores PIC y AVR.  

Ver: 

* [Pull up resistors](https://github.com/LDmicro/LDmicro/wiki/Pull-up-resistors)  
* [Disable Pull up resistors](https://github.com/LDmicro/LDmicro/wiki/Disable-Pull-up-resistors)
* [Pull down resistors](https://github.com/LDmicro/LDmicro/wiki/Pull-down-resistors)  

### COMPILACION  

Ahora puede generar código desde su programa. Elija Compilar -> Compilar, o Compilar -> Compilar Como ... si ha compilado previamente este programa y desea especificar un nombre de archivo de salida diferente (variante).  

Si no hay errores, LDmicro generarará un archivo Intel IHEX listo para la programación (descarga) en su microcontrolador.  

Utilice cualquier hardware y software de programación que disponga para cargar el archivo hexadecimal en el microcontrolador.  

¡Recuerde establecer los fusibles (bits) de configuración!  

Para los procesadores PIC16, los bits de configuración están incluídos en el fichero .hex, y la mayoría del software de programación automáticamente buscará allí su configuración.  
IMPORTANTE: Para procesadores AVR, debe establecer los bits de configuración manualmente.  

---

## REFERENCIA DE INSTRUCCIONES

Terminología:

* "Activado por Nivel": la salida lógica del objeto es controlada por el nivel lógico leído en la entrada del mismo (ON = 1; OFF = 0).  
* "Activado por Flanco": la salida lógica del objeto cambia sólo en en el instante en que la entrada lógica cambia de un valor a otro. El cambio puede ser activado por el flanco positivo (cambio de 0 a 1) o activado por el flanco negativo (cambio de 1 a 0) de la señal de entrada.  
* La mayoría de los objetos de LDmicro son "Activados por Nivel", algunos objetos son "Activados por Flanco".  

---

### CONTACTO NORMALMENTE ABIERTO

```
         Xname         Yname        Rname  
      ----] [----   ----] [----  ----] [----  
```
Esta instrucción examina el estado de un pin de entrada, un pin de salida, o un relé interno (marca). Si la señal en su entrada es 0 (OFF, False), entonces la señal en su salida es 0 (OFF, False). Si la señal en su entrada es 1 (ON, True), entonces la señal en su salida es 1 (ON). Si y sólo si el pin de entrada, pin de salida o relé interno está en 1 (ON, True), la salida será 1 (ON, True), de lo contrario será 0 (OFF, False).  

Instrucción Activada por Nivel.  

---

### CONTACTO NORMALMENTE CERRADO

```
        Xname         Yname         Rname  
      ---]/[----   ----]/[----   ----]/[----  
```
Esta instrucción examina el estado de un pin de entrada, un pin de salida, o un relé interno (marca). Si la señal en su entrada es 0 (OFF, False), entonces la señal en su salida es 1 (ON, True). Si la señal en su entrada es 1 (ON), entonces la señal en su salida es 0 (OFF). Si y sólo si el pin de entrada, pin de salida o relé interno está en 0 (OFF, False), la salida será 1 (ON, True), de lo contrario será 0 (OFF, False). Esta instrucción es la opuesta a la de un contacto normalmente abierto.  
Instrucción Activada por Nivel.  

---

### BOBINA NORMAL

```
         Yname         Rname  
      ----( )----   ----( )----  
```
Esta instrucción controla el estado de salidas físicas (pines) y relés internos (marcas) del microcontrolador. Si la señal en su entrada es 0 (OFF, False), entonces el estado en su salida es 0 (OFF, False). Si la señal en su entrada es 1 (ON, True), entonces el estado en su salida es 1 (ON, True).  

Esta instrucción debe ser siempre programada en el extremo derecho del escalón (rung). Instrucción Activada por Nivel.  

---

### BOBINA INVERTIDA  
```
         Yname         Rname  
      ----(/)----   ----(/)----  
```
Esta instrucción controla el estado de salidas físicas (pines) y relés internos (marcas) del microcontrolador. Si la señal en su entrada es 0 (OFF, False), entonces la señal en su salida es 1 (ON, True). Si la señal en su entrada es 1 (ON, True), entonces la señal en su salida es 0 (OFF, False). Esta instrucción es la opuesta a la de un una bobina normal.  

Esta instrucción debe ser siempre programada en el extremo derecho del escalón (rung). Instrucción Activada por Nivel.  

---

### ACTIVAR BOBINA (SET)  
```
         Yname         Rname  
      ----(S)----   ----(S)----  
```
Esta instrucción controla el estado de salidas físicas (pines) y relés internos (marcas) del microcontrolador.

Si la señal en su entrada lógica es 1 (ON, True), entonces el estado del relé interno o del pin de salida pasa a ser 1 (ON, True) y se mantiene en ese estado.
Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), el estado del relé interno o del pin de salida no cambia.  

Una vez que la bobina está activada (SET), el mantenimiento o la aparición de sucesivas señales en 1 en su entrada lógica no cambiarán el estado de la salida lógica de la bobina.  
Esta instrucción sólo puede cambiar el estado de salida de una bobina de 0 (OFF, False) a 1 (ON, True), por lo que es típicamente utilizada en combinación con una bobina RESET.  
Esta instrucción debe ser siempre programada en el extremo derecho del escalón (rung). Instrucción Activada por Nivel.  

---

### DESACTIVAR BOBINA (RESET)  
```
         Yname         Rname  
      ----(R)----   ----(R)----  
```
Esta instrucción controla el estado de salidas físicas (pines) y relés internos (marcas) del microcontrolador.

Si la señal en su entrada es 1 (ON, True), entonces el estado del relé interno o el pin de salida pasa a ser 0 (OFF, False) y se mantiene en ese estado.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), el estado del relé interno o del pin de salida no cambia.  

Una vez que la bobina está desactivada (RESET), el mantenimiento o la aparición de sucesivas señales en 1 en su entrada lógica no cambiarán el estado de la salida lógica de la bobina.

Esta instrucción sólo puede cambiar el estado de salida de una bobina de 1 (ON, True) a 0 (OFF, False), por lo que es típicamente utilizada en combinación con una bobina SET.  

Esta instrucción debe ser siempre programada en el extremo derecho del escalón (rung). Instrucción Activada por Nivel.  

---

### BOBINA BIESTABLE  
```
         Yname         Rname  
      ----(T)----   ----(T)----  
```
Esta instrucción controla el estado de salidas físicas (pines) y relés internos (marcas) del microcontrolador.  

Una Bobina T (Trigger) es un biestable (conmutador, flip-flop) gobernado por flanco positivo en la entrada lógica. El estado de la salida de la bobina cambia al estado opuesto ante cada flanco positivo (ascendente) leído en su entrada, es decir en el instante en que la señal en su entrada lógica pasa 0 (OFF, False) a 1 (ON, True), y mantiene ese nuevo estado hasta que se detecte un nuevo flanco ascendente en la señal de entrada.  

Esta instrucción debe ser siempre programada en el extremo derecho del escalón (rung). Instrucción Activada por Flanco (positivo).  

```
                          |   La duración de la señal 1 en la entrada
                          |   debe ser mayor al tiempo de ciclo del PLC                                                       
                          |
                T Trigger |     __       ___________      >_<  t > 1 PLC CYCLE  ___
                   input  | ___/  \_____/           \_____/ \__________________/
                          |    |        |                 |                    |
                T Trigger |    v________v                 v____________________v
                  output  | ___/        \_________________/                    \___
                        --+------------------------------------------------------> time
                          |
```

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

> **NOTA:**  
> Varias bobinas con el mismo 'YName' o 'RName' pueden ser mentalmente representadas como un circuito integrado por múltiples entradas y una única salida.  
> 
> Las bobinas normal e invertida transfieren inmediatamente el estado de su entrada (normal o invertida) a su salida.  
> Las bobinas S, R y T retienen el estado de su salida.  

> Las bobinas SET y RESET son activadas por nivel, la bobina T es activada por el flanco positivo en la señal de entrada.  
> Si antes de una bobina R, S se inserta la instrucción "OSR: ONE-SHOT RISING" o "OSF: ONE-SHOT FALLING", se obtiene un elemento activado por el flanco positivo o por el flanco negativo, respectivamente.  

> Si se usan sólo las instrucciones R y S, se obtiene el clásico disparador RS. Si se agrega la instrucción T, se obtiene el más reciente disparador RST. Puede utilizar varias bobinas R...R, o varias entradas S...S con un mismo nombre en un mismo programa. Puede utilizar cualquier combinación de entradas para una bobina de salida física 'YName' o para un relé interno 'RName'.  

---

### DETECCION DE FLANCO POSITIVO  
```
      ----[_/¨OSR_/¨\_]----  
```
Normalmente, la salida de esta instrucción es 0 (OFF, False).  

Si la entrada de la instrucción es 1 (ON, True) en un ciclo de programa y fue 0 (OFF, False) durante el ciclo anterior, entonces la salida es 1 (ON, True) durante un solo ciclo de programa.  

Por lo tanto, genera un pulso positivo con duración de un ciclo de PLC en cada flanco positivo de su señal de entrada. Esta instrucción es útil si desea activar eventos a partir del flanco ascendente de una señal.  

```
                  OSR     |     ______________________
                  input   | ___/                      \_______
                          |    |
                  OSR     |    >_< 1 PLC CYCLE
                  output  | ___/ \_____________________________
                          |
                        --+--------------------------------------> time
                          |
```


### DETECCION DE FLANCO NEGATIVO  
```
      ----[¨\_OSF_/¨\_]----  
```
Normalmente, la salida de esta instrucción es 0 (OFF, False).  

Si la entrada de la instrucción es 0 (OFF, False) en un ciclo de programa y fue 1 (ON, True) durante el ciclo anterior, entonces la salida es 1 (ON, True) durante un solo ciclo de programa.  

Por lo tanto, genera un pulso positivo con duración de un ciclo de PLC en cada flanco negativo de su señal de entrada. Esta instrucción es útil si desea activar eventos a partir del flanco descendente de una señal.  

```
                  OSR, OSF|     ______________________
                  input   | ___/                      \_______
                          |                           |
                  OSF     |                           >_< 1 PLC CYCLE
                  output  | __________________________/ \_____
                          |
                        --+--------------------------------------> time
                          |
```

---  

### DETECCION DE FLANCO NEGATIVO CON SALIDA INVERTIDA  
```
      ----[¨\_OSL¨\_/¨]----  
```
Normalmente, la salida de esta instrucción es 1 (ON, True).  

Si la entrada de la instrucción es 0 (OFF, False) en un ciclo de programa y fue 1 (ON, True) durante el ciclo anterior, entonces la salida es 0 (OFF, False) durante un solo ciclo de programa.  

Por lo tanto, genera un pulso negativo con duración de un ciclo de PLC en cada flanco negativo de su señal de entrada. Esta instrucción es útil si desea desactivar eventos a partir del flanco descendente de una señal.  
```

                  OSL     | ___           _______
                  input   |    \_________/
                          |    |
                          |    > < 1 PLC CYCLE
                  OSL     | ___   _______________
                  output  |    \_/
                        --+---------------------------> time
                          |

```

---  

### TEMPORIZADOR A LA CONEXION  
```
           Tname  
      --[TON 1.000 s]--  
```
Si la señal en la entrada lógica de la instrucción pasa de 0 (OFF, False) a 1 (ON, True), la señal en la salida permanece en 0 (OFF, False) durante el tiempo T antes de pasar a 1 (ON, True). Cuando  la señal en la entrada lógica de la instrucción pasa de 1 (ON, True) a 0 (OFF, False), la señal en la salida pasa a 0 (OFF, False) inmediatamente. La variable interna del temporizador se restablece a 0 cada vez que la entrada pasa a 0 (OFF, False); la señal en la entrada lógica debe permanecer verdadera durante al menos el tiempo T antes de que la salida resulte verdadera. El tiempo de retardo es configurable.  

La variable "Tname"cuenta desde cero en unidades de tiempo de ciclo del programa. La instrucción TON produce una salida True cuando la variable interna del temporizador es mayor igual o igual al tiempo de retardo configurable. Es posible manipular la variable interna del temporizador por medio de otras instrucciones en el programa, p. ej.por ejemplo con una instrucción MOV.  
```

                   ^  La duración de la señal 1 en la entrada
                   |  debe ser mayor o igual al parámetro: t(on) >= par
           TON     |     ____________         ___
           input   | ___/            \_______/   \_____
                   |    |            |
                   |    | t          |
                   |    |<---->|     |
                   |           |     |
                   |           v     v
           TON     |            ______
           output  | __________/      \________________
                 --+-----------------------------------> time,s
                   |
```

---

### TEMPORIZADOR A LA DESCONEXION  
```
           Tname  
      --[TOF 1.000 s]--  
```
Si la señal en la entrada lógica de la instrucción pasa de 0 (OFF, False) a 1 (ON, True), la señal en la salida pasa a 1 (ON, True) inmediatamente y permanece en ese estado mientras se mantiene en 1 la señal de entrada. Cuando  la señal en la entrada lógica de la instrucción pasa de 1 (ON, True) a 0 (OFF, False), la señal en la salida permanece en 1 (ON, True) durante el tiempo T antes de pasar a 0 (OFF, False). La variable interna del temporizador se restablece a 0 cada vez que la entrada pasa a 1 (ON, True). El tiempo de retardo es configurable.  

La variable "Tname"cuenta desde cero en unidades de tiempo de ciclo del programa. La instrucción TON produce una salida True cuando la variable interna del temporizador es mayor igual o igual al tiempo de retardo configurable. Es posible manipular la variable interna del temporizador por medio de otras instrucciones en el programa, p. ej.por ejemplo con una instrucción MOV.  
```
                   ^  La duración de la señal 1 en la entrada
                   |  debe ser mayor al tiempo de ciclo del PLC
           TOF     |        _               ___   ___________
           input   | ______/ \_____________/   \_/           \___________
                   |       | |             |                 |
                   |       | |   t         |                 |    t
                   |       | |<----->|     |                 |<----->|
                   |       |         |     |                         |
                   |       v         v     v                         v
           TOF     |        _________       _________________________
           output  | ______/         \_____/                         \___
                 --+-----------------------------------------------------> time,s
                   |
```

---

### TEMPORIZADOR A LA CONEXION RETENTIVO  
```
           Tname  
      --[RTO 1.000 s]--  
```
Esta instrucción hace un seguimiento de cuánto tiempo su entrada de señal lógica ha estado en 1 (ON, True), sumando los tiempos parciales de activación. Si la señal en su entrada lógica ha sido 1 (ON, True) para una sumatoria de tiempos igual o mayor a el parámetro T, entonces la salida pasa a 1 (ON, True). De lo contrario, la salida es 0 (OFF, False).  

La entrada no requiere estar activada en forma continua por el tiempo T, p. ej.  si el parámetro T = 2 s y la entrada es 1 durante 0,6 s, luego 0 durante 5,0 s, y luego 1 durante 1,4 s, entonces la salida será 1.  

Una vez que que la salida está activada en 1 (ON, True), permanecerá en ese estado incluso después de que la entrada vuelva a 0 (OFF, False), siempre y cuando la señal en la entrada haya sido 1 (ON, True) por un período total mayor al tiempo T. El tiempo de retardo es configurable.  
Este temporizador debe ser restablecido programáticamente, utilizando la instrucción de RESET Timer/Counter --{RES}--.  

La variable "Tname" cuenta desde cero en unidades de tiempo de ciclo del programa. Es posible manipular la variable interna del temporizador por medio de otras instrucciones en el programa, p. ej.por ejemplo con una instrucción MOV.  

```
                   ^  La suma de los tiempos de los pulsos en 1 debe ser  
                   |  mayor o igual al parámetro t: t1 + t2+ .. +tn >= par
                   |
           RTO     |     __     _    _________          
           input   | ___/  \___/ \__/         \________________
                   |    |  |   | |  |
                   |    |t1|   | |  |tn                   Trto
                   |    |<>|  >| |< |<>|                  RESET
                   |           t2      |                  |
                   |                   v                  v
           RTO     |                    __________________
           output  | __________________/                  \___
                 --+-------------------------------------------> time,s
```

---
### TEMPORIZADOR A LA DESCONEXION RETENTIVO  
```
            Tname  
        --[RTL 1.000 s]--  
```
Esta instrucción hace un seguimiento de cuánto tiempo su entrada de señal lógica ha estado en 0, (OFF, False), sumando los tiempos parciales de desactivación.  Si la señal en su entrada lógica ha sido 0 (OFF, False) para una sumatoria de tiempos igual o mayor a el parámetro T, entonces la salida pasa a 1 (ON, True). De lo contrario, la salida es 0 (OFF, False).  

La entrada no requiere estar desactivada en forma continua por el tiempo T, p. ej.  si el parámetro T = 2 s y la entrada es 0 durante 0,6 s, luego a durante 5,0 s, y luego 0 durante 1,4 s, entonces la salida será 1.  

Una vez que que la salida está activada en 1 (ON, True), permanecerá en ese estado incluso después de que la entrada vuelva a 1 (ON, True), siempre y cuando la señal en la entrada haya sido 0 (OFF, False) por un período total mayor al tiempo T. El tiempo de retardo es configurable.  
Este temporizador debe ser restablecido programáticamente, utilizando la instrucción de RESET Timer/Counter --{RES}--.  

La variable "Tname" cuenta desde cero en unidades de tiempo de ciclo del programa. Es posible manipular la variable interna del temporizador por medio de otras instrucciones en el programa, p. ej.por ejemplo con una instrucción MOV.  

```
                   ^  La suma de los tiempos de los pulsos en 0 debe ser
                   |  mayor o igual al parámetro t: t1 + t2+ .. +tn >= par
                   |
           RTL     | ___    ___   __           ________
           input   |    \__/   \_/  \_________/
                   |    |  |   | |  |
                   |    |t1|   | |  |tn                   Trtl
                   |    |<>|  >|-|< |<>|                  RESET
                   |           t2      |                  |
                   |                   v                  v
           RTL     |                    __________________
           output  | __________________/                  \___
                 --+-------------------------------------------> time,s
```
---

### TEMPORIZADOR CICLICO  
```
             Tname  
      --[TCY 500 ms]--  
```
Si la señal en su entrada lógica es 1 (ON, True), esta instrucción produce en su salida una señal alternada ON->OFF->ON->OFF->..., es decir un ciclo con período T y frecuencia 1/T Hz. Si la señal de entrada 0 (OFF, False), entonces la señal de salida es 0 (OFF, False).  

Si el valor Tname es igual al tiempo de ciclo del programa, el ciclo en la salida del TCY es igual a OSC (período de ciclo del PLC). El tiempo de retardo es configurable.  

```
                   ^  La duración de la señal 1 en la entrada
                   |  debe ser mayor al parámetro: t > par
           TCY     |     ______________________
           input   | ___/                      \_______
                   |    |                      |
                   |    |  1s    1s    1s      |
                   |    |<--->|<--->|<--->|    |
                   |    |     |     |     |    v
           TCY     |    v   __|   __|   __|   _
           output  | ______/  \__/  \__/  \__/ \_______
                 --+----------------------------------------> time,s
                   |
```
---

### OSCILADOR  
```
    --[_/¨OSC_/¨\_/¨\_]--  
```
F=1/(2*Tcycle)  

Si la señal en la entrada lógica de la instrucción es 1 (ON, True), produce un ciclo con frecuencia igual al inverso del tiempo de ciclo del PLC * 2; es decir que la salida de la instrucción estará inaactiva (0, OFF) durante un ciclo de PLC, y activa (1, ON) durante el ciclo siguiente.  

Si la señal en la entrada de la instrucción es 0 (OFF, False),la señal de salida siempre es 0 (OFF, False).  

---

### TEMPORIZADOR DE IMPULSO POSITIVO  
```
         Tname  
    --[THI 1.000 s]--  
```
Si en la entrada lógica de la instrucción la señal tiene un flanco positivo (ascendente) pasando de 0 (OFF, False) a 1 (ON, True), la señal en la salida pasará inmediatamente de 0 (OFF, False) a 1 (ON, True).  

Permanecerá en ese estado durante el tiempo T, luego pasará nuevamente a 0 (OFF, False) y se mantendrá en ese estado sin importar el estado en que se mantiene la entrada (0 - 1), hasta la próxima ocurrencia de un flanco positivo. El tiempo de retardo es configurable.  
Instrucción Activada por Flanco (positivo).  
```
                   ^  La duración de la señal 1 en la entrada
                   |  debe ser mayor al tiempo de ciclo del PLC
           THI     |     _           ________
           input   | ___/ \_________/        \_______
                   |    |           |
                   |    |  t        | t
                   |    |<-->|      |<-->|
                   |    |    |      |    |
                   |    v    v      v    v
           THI     |     ____        ____
           output  | ___/    \______/    \___________
                 --+------------------------------------> time,s
                   |

```

---

### TEMPORIZADOR DE IMPULSO NEGATIVO  
```
         Tname  
    --[TLO 1.000 s]--  
```
Si en la entrada lógica de la instrucción la señal tiene un flanco negativo (descendente) pasando de  1 (ON, True) a 0 (OFF, False), la señal en la salida pasará inmediatamente de 1 (ON, True) a  0 (OFF, False).  

Permanecerá en ese estado durante el tiempo T, luego pasará nuevamente a 1 (ON, True) y se mantendrá en ese estado sin importar el estado en que se mantiene la entrada (0 - 1), hasta la próxima ocurrencia de un flanco negativo. El tiempo de retardo es configurable.  
Instrucción Activada por Flanco (negativo).  
```
                   ^  La duración de la señal 0 en la entrada
                   |  debe ser mayor al tiempo de ciclo del PLC
           TLO     | ___   _________          _______
           input   |    \_/         \________/
                   |    |           |
                   |    |  t        |  t
                   |    |<-->|      |<-->|
                   |    |    |      |    |
                   |    v    v      v    v
           TLO     | ___      ______      ___________
           output  |    \____/      \____/
                 --+------------------------------------> time,s
```

---

### RESET TEMPORIZADOR RETENTIVO  
```
     RTOname     RTLname  
    --{RES}--   --{RES}--  
```
Esta instrucción restablece la variable interna (pone a a 0) de un temporizador a la conexión retentivo (RTO), o a la desconexión retentivo (RTL).  

Los temporizadores TON y TOF son restablecidos automáticamente cuando la señal en la entrada lógica es 0 ó 1, respectivamente, por lo que la instrucción RES no es necesaria para estos temporizadores.  
Las variables internas de las instrucciones RTO y RTL no son reseteadas automáticamente, por lo que deben restablecerse programáticamente utilizando una instrucción RES. Cuando la entrada  de la instrucción RES es 1 (ON, True), el temporizador se restablece; cuando la entrada es 0 (OFF, False), no se ejecuta ninguna acción.  

RES restablece la variable numérica de las instrucciones de RTO, RTL.  
Si la salida del temporizador retentivo está activada en 1 (ON, True), al restablecerse la variable a 0 mediante la instrucción RES la salida también será inmediatamente restablecida a 0 (OFF, False).  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---
### RETARDO  
```
       n us  
    --[DELAY]--  
```
La instrución RETARDO causa una interrupción por un tiempo equivalente a n microsegundos en la ejecución del programa.  

La instrucción RETARDO no utiliza ningún temporizador ni contador.  
Las operaciones básicas del microcontrolador NOP() y JMP(dirección actual + 1) son utilizadas para generar la interrupción.  

Ver [DELAY-us](https://github.com/LDmicro/LDmicro/wiki/DELAY-us)  

---


### CONVERSOR TIEMPO A RETARDO  
```
          Tconst  
    --[T2DELAY 10 us]--  
```
La instrucción Conversor Tiempo a Retardo toma la constante de tiempo de ciclo T en ms, la convierte en un valor de retardo específico y guarda el resultado en la variable Tconst para su posterior utilización como parámetro en la instrucción RETARDO.  
  
---
### Actualización sobre temporizadores: versiones >= v.4.4.1  
### TON, TOF, THI, TLO, RTO, RTL, TCY  
```
          Tname  
    --[TXX variable]--  
```
**Se puede utilizar una variable general como parámetro de un temporizador.**  
Usted debe calcular el valor correcto de la variable de acuerdo al tiempo de ciclo del PLC previamente establecido en Configuraciones > MCU parámetros... > Tiempo Ciclo.  

> Parámetro (ms) = Tplc (ms) * variable.  
P. ej. si Tiempo Ciclo (ms) = 10, para establecer un parámetro de 25 segundos en un temporizador la variable deberá tener un valor igual a 2500 (Tpar = 0,01 ms * 2500).  

---

### CONTADOR ASCENDENTE / DESCENDENTE  
```
        Cname            Cname  
    --[CTU >= 5]--   --[CTD > -5]--  
```
Una transición de 0 (OFF, False) a 1 (ON, True) (flanco positivo) en la señal de entrada lógica del contador CTU (ascendente) o del contador CTD (descendente) modifica de la variable de conteo, incrementando o decrementando su valor.  
El estado de la salida del contador es 1 (ON, True) si y sólo si la variable de conteo es igual o mayor que el parámetro en CTU, o mayor en CTD, y 0 (OFF, False) en caso contrario.  
El estado de la salida puede ser 1 incluso si la señal en la entrada es 0; sólo depende de la comparación entre la variable interna del contador y el parámetro.  

Una vez que en el contador la variable alcanza el valor requerido para la activación de la salida (parámetro), el conteo (ascendente o descendente) no se continúa aunque aparezcan nuevos flancos positivos en la entrada lógica del contador.  

Se pueden programar instrucciones CTU y CTD con el mismo nombre de variable Cname, con el fin de incrementar y decrementar el mismo contador.  

La instrucción RES (Cname) reinicia un contador (pone a 0 su variable), y tiene prioridad respecto a la entrada de señal lógica, es decir que mientras RES (Cname) esté en 1 (ON, True), el valor de conteo permanecerá en 0 en CTU, o en el valor del parámetro en CTD, y ninguna señal en su entrada lo modificará.  

Se pueden realizar también operaciones aplicables a variables generales (aritméticas, mover, comparación, etc.) sobre la variable de conteo.  

```
                  |     ___      ________        __       ____            _____
            Input |____/   \____/        \______/  \_____/    \__________/
                  |
                  |    |        |               |        |               |
                  |    v        v               v        x               v
                  |
          CTU (3) | 0  1        2               3        3       0       1
                  |                                              _____
              RES |_____________________________________________/     \_________
                  |                              _______________
              OUT |_____________________________/               \_______________
                  |
                  |    |        |               |         |               |
                  |    v        v               v         v               x
                  |
                  |
          CTD (4) | 4  3        2               1         0          4    4
                  |                                                   ________
              RES |__________________________________________________/        \__
                  |                                        __________
              OUT |_______________________________________/          \___________
                  |

```

> **NOTA**:  
> Si se desea realizar conteo ascendente con una variable que pueda sobrepasar por exceso el parámetro (var > par) y/o conteo descendente y sobrepasar por defecto el parámetro (var < par), deberá implementarse por medio de rutinas generadas con instrucciones matemáticas (suma y resta), de evaluación de flanco, y de comparación contra el parámetro fijo o variable. P. ej.:

```
  ||; Increasing variable = Count Up
  ||       Xinc                        { ADD var :=}
  ||-------] [------[_/¨OSR_/¨\_]------{ var + 1   }--
  ||  
  ||; Decreasing variable = Count Down
  ||       Xdec                        { SUB var :=}
  ||-------] [------[_/¨OSR_/¨\_]------{ var - 1   }--
  ||
  ||; Evaluating the math counter output (fixed/variable parameter)
  ||       [ var  ]                           Rreached
  ||-------[>= par]------------------------------( )--
  ||
  ||; Resetting the math counter
  ||       Xres                         { var  :=  }
  ||-------] [--------------------------{ 0     MOV}--
```
  
---

### RESET CONTADOR ASCENDENTE / DESCENDENTE  
```
     CTUname     CTDname  
    --{RES}--   --{RES}--  
```
Esta instrucción restablece al valor 0 la variable interna en un contador ascendente (CTU), y restablece al valor inicial la variable interna en un contador descendente (CTD).  

Las variables internas de las instrucciones CTU y CTD no son reseteadas automáticamente, por lo que deben restablecerse programáticamente utilizando una instrucción RES.  

Cuando la entrada  de la instrucción RES es 1 (ON, True), el contador se restablece; cuando la entrada es 0 (OFF, False), no se ejecuta ninguna acción.  

RES restablece sólo la variable numérica de las instrucciones de CTU / CTD, no sus salidas. El estado de las salidas sólo depende de la operación de comparación entre la variable y el parámetro.

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  
  
---

### CONTADOR CIRCULAR ASCENDENTE / DESCENDENTE  
```
        Cname           Cname  
    --[CTC 0:7]--   --[CTR 5:0]--  
```
Un contador circular funciona como un contador normal ascendente o descendente, excepto que después de alcanzar su límite (variable igual al parámetro), al leer el primer flanco ascendente en su entrada, restablece automáticamente su variable a 0 (en CTC) o al valor máximo (en CTD).  

Al ocurrir este evento, su salida se activa durante un sólo ciclo de escaneo de programa (pulso positivo).  

Por ejemplo, el contador CTC mostrado en primer lugar contaría 0, 1, 2, 4, 5, 6, 7, 0, 1, 2,...   Esto es útil en combinación con operaciones de comparación ejecutadas sobre la variable "Cname"; usted puede utilizar ésto como un secuenciador para activar o desactivar otras instrucciones o rutinas del programa.  

CTR es un contador circular reverso, el contador CTR mostrado en segundo lugar contaría 5, 4, 3, 2, 1, 0, 5, 4, 3,...  

```
                  |     ___      ________        __       ____             ____
            Input |____/   \____/        \______/  \_____/    \___________/
                  |
                  |    |        |               |        |                |
                  |    v        v               v                         v
                  |
           CTC(3) | 0  1        2               3         0               1
                  |                             >_< 1 PLC CYCLE
              OUT |_____________________________/ \_____________________________
                  |
                  |    |        |               |         |                |
                  |    v        v               v         v                v
                  |
                  |
           CTR(4) | 4  3        2               1         0                4
                  |                                       >_< 1 PLC CYCLE  
               OUT|_______________________________________/ \___________________
                  |

```
La instrucción RES (Cname) reinicia un contador (pone a 0 su variable), y tiene prioridad respecto a la entrada de señal lógica, es decir que mientras RES (Cname) esté en 1 (ON, True), el valor de conteo permanecerá en 0 en CTC, o en el valor del parámetro en CTR, y ninguna señal en su entrada lo modificará.  

Se pueden realizar también operaciones aplicables a variables generales (aritméticas, mover, etc.) sobre la variable de conteo.  

---

### Actualización sobre contadores: versiones >= v.4.4.1  
### CTU, CTD, CTC, CTR  

**Se puede utilizar una variable general como parámetro de un contador.**  

---

### CONVERSOR TIEMPO A CONTADOR  

```
         Tnew  
    --[T2CNT 10 ms]--  
```
La instrucción  Conversor Tiempo a Contador toma la constante de tiempo de ciclo de PLC en ms, la convierte en un valor de unidades en los distintos temporizadores y guarda el resultado en la variable Tnew para su posterior utilización como parámetro en contadores.  

Ver [TIME-to-COUNTER-converter](https://github.com/LDmicro/LDmicro/wiki/TIME-to-COUNTER-converter)

---

### Actualización sobre contadores: versiones >= v.4.4.0  
Los contadores CTU, CTD, CTC y CTR en las versiones >= v.4.4.0 admiten más opciones de configuración:  

1. Se puede utilizar una variable general como parámetro de un contador.  
2. Permiten establecer un valor de inicialización START establecido por el usuario y distinto de 0 (valor por defecto), es decir que al efectuar la instrucción RES sobre un contador, la variable contendrá el valor específico START.  
En CTU y CTC (contadores ascendentes), START deberá ser menor que el parámetro máximo (START < MAX); en CTD y CTR (contadores descendentes), START deberá ser mayor que el parámetro mínimo (START > MIN).  
3. También permiten establecer si la señal a evaluar en la entrada de conteo será dinámica por flanco positivo (/) o flanco negativo (\\), o será estática normal (-) o invertida (o).  
4. Los contadores circulares CTC y CTD ya no requieren ser programados en el extremo derecho del escalón (rung), pudiendo utilizarse como operaciones intermedias. P. ej. ésto es útil si envía la señal de salida del contador a un relé interno (marca) Rname y utiliza sus contactos Rname como condición de entrada para otras instrucciones.  

```
   ||                               ||
   ||  X1          CTU1:0      Y1   ||
   ||--] [---+---/[CTU>=10]----( )--|| '/' Entrada dinámica, activa en transición 0 a 1.
   ||        |                      ||
   ||        |     CTU2:0      Y2   ||
   ||        +---\[CTU>=10]----( )--|| '\' Entrada dinámica, activa en transición 1 a 0.
   ||        |                      ||
   ||        |     CTU3:0      Y3   ||
   ||        +----[CTU>=10]----( )--|| '-' Entrada estática directa, activa en 1.
   ||        |                      ||
   ||        |     CTU4:0      Y4   ||
   ||        +---o[CTU>=10]----( )--|| 'o'  Entrada estática inversa, activa en 0.
   ||                               ||      Negación en la entrada lógica.
   ||                               ||      Nivel 0 externo produce nivel 1 interno.
   ||                               ||

```

> **NOTA:**
> Todos los contadores en versiones anteriores a v.4.4.0 tienen entrada dinámica por flanco positivo. La entrada por defecto --/{CTX}-- es compatible con dichas versiones.  

> **Consejo:**  
> Si para la aplicación resultan necesarios contadores bidireccionales con posibilidad de que la variable de conteo vaya más allá de los valores de parámetros (por exceso o por defecto), este requerimiento puede implementarse en el programa por medio de instrucciones aritméticas de suma y resta sobre una misma variable, en conjunto con operaciones de comparación.  

Puede utilizar la instrucción de evaluación de flanco positivo -{OSR}- para emular los contadores de versiones anteriores a v.4.4.0 al realizar esta programación.  

Puede implementar la operación -{MOV 0, var}- en forma equivalente a una operación -{RES Cnt}-  

---

### CONVERTIDOR ANALOGICO/DIGITAL  

```
        Aname  
    --{READ ADC}--  
```
LDmicro puede generar código para usar los conversores Analógico/Digital incorporados en ciertos modelos de microcontroladores.  

Si la señal en la entrada lógica a esta instrucción es 1 (ON, True), entonces se adquiere una sola muestra del conversor A/D, y su valor es almacenado en la variable "Aname".  

Si la señal en la entrada lógica es 0 (OFF, False), entonces la variable "Aname" no tendrá cambios. 

Para todos los dispositivos actualmente soportados, una señal de 0 voltios corresponde a una lectura ADC de 0, y una señal igual a Vdd (la tensión de alimentación) corresponde a una lectura ADC de 1023.

Si está utilizando un microcontrolador AVR, conecte AREF a AVcc, y AVcc a Vcc con un circuito de filtrado LC (ver hoja de datos del microcontrolador).  

> **NOTA:**  A partir de la versión v.4.4.0, puede establecer una tensión de referencia diferente de Vdd mediante el parámetro REFS de la instrucción READ ADC.  

El parámetro REFS por defecto 0 es compatible con versiones anteriores, y utiliza Vdd como tensión de referencia para la conversión A/D.  

Puede seleccionar otras fuentes de referencia en función del modelo de microcontrolador.  
Ver [ADC-Voltage-Reference](https://github.com/LDmicro/LDmicro/wiki/ADC-Voltage-Reference)  

Esta variable puede ser manipulada con las mismas operaciones que se pueden efectuar sobre variables generales (comparaciones, aritméticas, y así sucesivamente).  

Puede utilizar operaciones matemáticas para convertirla a unidades más convenientes (unidades de ingeniería), pero recuerde que estará utilizando operaciones aritméticas sobre enteros.  

Asigne un pin a la variable "Aname" de la misma manera que asigna un pin a una entrada o salida digital, haciendo doble click en su nombre en la lista de objetos en la parte inferior de la ventana.  

En general no todos los pines estarán disponibles para uso con el convertidor A/D. El software no le permitirá asignar pines que no diponen de ADC a una entrada analógica.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### MODULACION DE ANCHO DE PULSO (PWM): CONFIGURAR CICLO DE SERVICIO

```  
       duty_cycle  
    --{PWM 10 kHz}--  
```
LDmicro puede generar código para usar el periférico PWM incorporado en ciertos microcontroladores.  
Si la condición de entrada a esta instrucción es 1 (ON, True), entonces la relación "marca" (1, ON) / "espacio" (0, OFF) en la modulación de ancho de pulso se ajusta al valor de la variable duty_cycle (ciclo de servicio).  

El ciclo de servicio debe ser un número entre 0 y 100 (porcentaje); 0 corresponde a siempre bajo (0, OFF) y 100 corresponde a siempre alto (1, ON). Si está familiarizado con el funcionamiento del periférico PWM, note que ésto significa que LDmicro escala automáticamente la variable del ciclo de trabajo a la frecuencia PWM en base al porcentaje establecido.  

Puede especificar la frecuencia base del PWM expresada en Hz. La frecuencia base que se especifique puede no ser exactamente alcanzable, dependiendo de cómo se divide en relación a la frecuencia de trabajo del microcontrolador (interna, RC, cristal). LDmicro seleccionará la frecuencia base más cercana posible; si el error es grande entonces lo advertirá en un cuadro de diálogo. Frecuencias base muy altas pueden sacrificar la resolución de la salida PWM.  

> **NOTA:**  
> El código generado por la lógica Ladder utiliza un temporizador interno para medir el tiempo de ciclo (ejecución) del programa. Ésto significa que la instrucción modulación de ancho de pulso PWM sólo está disponible para microcontroladores que disponen al menos de dos temporizadores internos (Timer0, Timer1). PWM utiliza el pin CCP2 (no CCP1) en microcontroladores PIC16, y el pin OC1B (no OC1A) en AVR.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

#### Actualización sobre PWM: versiones >= v.4.0.0  
LDmicro >= V4.x.x permite diferentes resoluciones en la variable duty_cycle:  
1. 0-100 - 6,7 bits  
2. 0-255 - 8 bits  
3. 0-511 - 9 bits  
4. 0-1023 - 10 bits  
También permite utilizar todos los PWM disponibles en el hardware.  
Ver [LDmicro-PWM](https://github.com/LDmicro/LDmicro/wiki/LDmicro-PWM)  

ATENCION: si se requiere cambiar la frecuencia base del PWM (no su ciclo de servicio) durante la ejecución del programa en el microcontrolador, utilice la instrucción RESET PWM siguiente.  

---

### MODULACION DE ANCHO DE PULSO (PWM): RESTABLECER

```  
       PWM  
    --{RES}--  
```
Si la señal en la entrada lógica de la instrucción es 1 (ON, True), la instrucción inhabilita la modulación de ancho de pulso PWM y establece la salida de pulsos en nivel bajo (0, OFF).  

Esta instrucción permite configurar otra frecuencia base para la modulación de ancho de pulso PWM, previamente establecida mediante una instrucción SET PWM DUTY CYCLE; esta nueva configuración sólo se puede implementar mientras el PWM está deshabilitado mediante la ejecución de la instrucción RESET PWM.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### TREN DE IMPULSOS

```
      [PULSER  counter   Ypulse]->  
    --[   D1  D0 accel-decel   ]--  
```

Si la señal en la entrada lógica de la instrucción es 1 (ON, True), esta instrucción genera un tren de impulsos en el pin de salida Ypulse.  

El campo Pulse counter establece la cantidad total de impulsos que se generarán; los campos Duration of 1 y Duration of 0 establecen la duración, medida en ciclos de PLC, del estado 1 (ON, True) y 0 (OFF, False) de cada impulso; el campo Accel/Decel Factor establece la aceleración y deceleración en el tren de impulsos y será un múltiplo de la duración de cada ciclo de PLC, con una tasa de cambio de +1 y -1 respectivamente; la variable Ypulse establece el pin de salida del tren de impulsos; la salida lógica de esta instrucción estará en 1 (ON, True) durante la ejecución del tren de impulsos.  

---

### PASO A PASO  

```
      [STEPPER step stepMax  Ystep]->  
    --[P                    100  1]---  
```

**Instrucción en preparación**

---

### VARIABLE PERSISTENTE  
```
      saved_var  
    --{PERSIST}--  
```
Cuando la condición de entrada de esta instrucción es 1 (ON, True), la variable especificada es automáticamente guardada (escrita) en la memoria EEPROM. Ésto significa que su valor se mantendrá incluso cuando el microcontrolador no está energizado (sin alimentación).  
No hay necesidad de guardar explícitamente la variable en EEPROM; ésto sucederá automáticamente siempre que cambie su valor.  

Después de la energización (alimentación) del microcontrolador, el valor guardado en la EEPROM se carga automáticamente en la variable en RAM.  

> **NOTA:** Si una variable que cambia con frecuencia se hace persistente, la memoria EEPROM en el microcontrolador puede degradarse muy rápidamente, dado que sólo soporta un número limitado de escrituras (~100.000).  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

Ver [MAKE-PERSISTENT-operation](https://github.com/LDmicro/LDmicro/wiki/MAKE-PERSISTENT-operation)  

---

### MOVER VARIABLE  
```
    {destvar :=  }     {Tret :=  }  
    --{ 123 MOV}--   --{srcvar MOV}--  
```
Si la señal en la entrada lógica de la instrucción 1 (ON, True), a la variable destino le será asignado un valor igual a la variable origen o de la constante. Cuando la entrada a esta instrucción es 0 (OFF, False), la variable destino permanece sin cambios.  

Con la instrucción MOV se puede asignar un valor a cualquier tipo de variable destino; ésto incluye variables de temporizadores y contadores, que pueden ser distinguidas en el esquema Ladder por comenzar con las letras 'T' o 'C'. Por ejemplo, una instrucción MOV 0 a "Tret" es equivalente a una instrucción Reset -{RES}- para ese temporizador.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### OPERACIONES ARITMETICAS CON VARIABLES  
```
      {ADD kay   :=}       {SUB Ccnt  :=}  
    --{ 'a' + 10   }--   --{ Ccnt - 10  }--  

      {MUL dest  :=}       {DIV dv    :=}  
    --{ var * -990 }--   --{ dv / -10000}--  
```
Si la señal en la entrada lógica en cada una de estas instrucciones es 1 (ON, True), la variable destino será igual a la operación especificada entre los operandos.  

Los operandos pueden ser variables (incluyendo variables de temporizador y contador) o constantes. Estas instrucciones utilizan variables con signo y sólo admiten números enteros. 

Una operación aritmética admite tener como variable destino la misma variable de origen.  

Recuerde que el resultado se evalúa en cada ciclo cuando la condición de entrada es 1 (ON, True).

Si está incrementando o decrementando una variable (es decir, si la variable destino es también uno de los operandos), entonces usted probablemente no querrá que suceda ésto; típicamente deberá utilizar una instrucción de detección de flanco previa para que la instrucción se evalúe sólo al detectarse un flanco ascendente o descendente en su entrada lógica.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  


**NOTA IMPORTANTE:**  
La instrucción DIV (dividir) trunca el valor del resultado, es decir que descarta los decimales; 7/3 = 2. Recuerde que las variables siempre son números enteros con signo.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### MODULO  
```
     {MOD dest:=}  
    --{src % 2}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), la variable destino será igual al resto (sobrante, número entero positivo) de la división del Operando1 por el Operando2.

Ejemplo: 7 % 3 = 1  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

Ver [Modulo_operation](https://en.wikipedia.org/wiki/Modulo_operation)  

---

### NEGATIVO  
```
      {NEG dest:=}  
    --{  -    src}--  
```
Si la señal en la entrada lógica de la instrucción es 1 (ON, True), esta instrucción invierte el signo de una variable (entero).

Negativo (NEG a:= -a)- es el equivalente optimizado de -{SUB a:= 0 - a}-  
Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### OPERACIONES DE COMPARACION
```  
      [var ==]       [var >]       [1 >=]  
    --[ var2 ]--   --[ 1   ]--   --[ Ton]--  

      [var !=]       [-4 <   ]       [1 <=]  
    --[ var2 ]--   --[ vartwo]--   --[ Cup ]--  
```
Esta instrucción se puede utilizar para comparar una variable con otra variable, o comparar una variable con una constante con signo. 

Si la señal en la entrada lógica de la instrucción es 1 (ON, True), entonces la salida es 1 (ON, True) si y solamente si el resultado de la comparación es verdadero.  

Las operaciones de comparación disponibles son: igual que, mayor que, mayor que o igual a, no es igual a, menor que, menorque o igual a.  

Si la entrada de señal de la instrucción es 0 (OFF, False), la operación de comparación no se efectúa y la salida es 0 (OFF, False).  

---

### OPERACIONES LOGICAS BIT A BIT
```  
      {AND  var1 :=}       {OR   Ccnt :=}  
    --{var2 & var3 }--   --{ Ccnt | 0o07}--  
  
      {XOR  dest :=}       {NOT  dv :=  }  
    --{ var ^ 0xAA }--   --{ ~0b11001100}--  
```
Si la señal en la entrada lógica de la instrucción es 1 (ON, True), la variable destino será igual a la operación lógica ejecutada entre los bits correspondientes (idéntica posición) de los operandos.  

Los operandos pueden ser variables (incluyendo variables de temporizador o contador) y/o constantes.  
Tenga en consideración que el resultado se evalúa en cada ciclo de programa mientras la condición de entrada sea verdadera.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable destino no modifica su valor.  

Ver [Bitwise_operation](http://en.wikipedia.org/wiki/Bitwise_operation)  

---

### ACTIVAR / DESACTIVAR UN BIT  
```
          {var}              {var}  
    --{SetBit bit}--   --{ClrBit bit}--  
```

`--{SetBit var, bit}--` activa (pone a 1) el bit número "bit" de la variable "var".  

`--{ClrBit var, bit}--` desactiva (pone a 0) el bit número "bit" de la variable "var".  

En la instrucción, "bit" significa número de bit, no máscara de bit.  

Por ejemplo, para establecer (poner a 1) el bit número 4 en la variable "var":  

```
    Bit número:  76543210  
    variable:    xxx1xxxx  
```

Se debe programar  
```
         {var}  
    --{SetBit 4}--   
```
ésto es equivalente a programar  
```
      {OR var  :=}
    --{var | 0x10}--
```
**NO** se deben utilizar hexadecimales para indicar el bit correspondiente, al programar instrucciones SetBit o ClrBit:  
```
          {var}  
    --{SetBit 0x10}-  NO ADMITIDO!!!  
```

---

### COMPROBAR EL ESTADO DE UN BIT  
```
           [var]                [var]  
    --[IfBitSet bit]--   --[IfBitClr bit]--  
```
Estas instrucciones se pueden usar para verificar el estado (0 = CLEAR; 1 = SET) de un bit determinado en una variable.  

Importante: "bit" significa número del bit, no máscara de bit.  

Si la señal en entrada lógica de la instrucción es 1 (ON, True), la salida es 1 (ON, True) si y sólo si el resultado de la comprobación es verdadero.
 
Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la operación de comparación no se efectúa y la salida es 0 (OFF, False).  

La operación --{IfBitSet var, 0}-- es equivalente a la expresión "si la variable es impar, entonces...".  
La operación --{IfBitClr var, 0}-- es equivalente a la expresión "si la variable es par, entonces...".  

---

### INSTRUCCIONES DE DESPLAZAMIENTO DE BITS: SHL, SHR, SR0, ROL, ROR  

Si la señal en la entrada lógica de la instrucción es 1 (ON, True), los bits de la variable origen son desplazados en la dirección correspondiente y asignados a la variable destino. La variable destino puede ser la misma que la variable origen.  

Los operandos que indican la cantidad de posiciones del desplazamiento pueden ser variables (incluyendo variables de temporizador o contador) o constantes.  

Tenga en consideración que el resultado se evalúa en cada ciclo de programa mientras la condición de entrada sea verdadera.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable destino no modifica su valor.  

> **Nota referida a los gráficos de las operaciones:**  
> MSB = Most Significant Bit (bit más significativo o de mayor peso)  
> LSB = Least Significant Bit (bit menos significativo o de menor peso)

Ver [Circular shift](https://en.wikipedia.org/wiki/Circular_shift)  
Ver [Arithmetic_shift](https://en.wikipedia.org/wiki/Arithmetic_shift)  

---

### DESPLAZAMIENTO ARITMETICO A LA IZQUIERDA / DERECHA

```  
     {SHL  var1 :=}     {SHR   cnt :=}  
    --{var2 << 2 }--   --{cnt >>  1 }--  
```
SHL - desplazamiento aritmético a la izquierda  
```
        MSB                    LSB  
            MSB-1            1  0  
    C <- x <- x <- .... <- x <- x <- 0  
```
SHR - desplazamiento aritmético a la derecha  
```
   MSB                         LSB  
        MSB-1              1    0  
    x--> x -> x -> .... -> x -> x -> C  
     \<</   
```
---

### DESPLAZAMIENTO LOGICO A LA DERECHA  
```
      {SR0  dest :=}  
    --{var  sr0  3 }--  
```
SR0 - desplazamiento lógico a la derecha  
```
        MSB                    LSB   
             MSB-1         1    0  
    0 -> x -> x -> .... -> x -> x -> C  
```

---
### DESPLAZAMIENTO LOGICO A LA IZQUIERDA  

El desplazamiento lógico a la izquierda es equivalente a SHL, desplazamiento aritmético a la izquierda.  

Ver [Logical shift](https://en.wikipedia.org/wiki/Logical_shift)  

---

### DESPLAZAMIENTO CIRCULAR A LA IZQUIERDA / DERECHA  
```
     {ROL  dest :=}    {ROR    dv :=}  
    --{var rol 4}--   --{var ror 4}--  
```

ROL - desplazamiento circular a la izquierda  
```
        MSB                    LSB  
             MSB-1         1    0  
    C <- x <- x <- .... <- x <- x  
          \>------------------>/  
```
  
ROR - desplazamiento circular a la derecha  
```
   MSB                    LSB  
        MSB-1        1    0  
    x -> x -> .... -> x -> x -> C  
     \<------------------</  
```

---

### INVERTIR ORDEN DE BITS  
```
         {dest:=}  
    --{OPPOSITE src}--  
```
Esta instrucción invierte el orden de todos los bits de una variable.  

Si la señal en la entrada lógica de la instrucción es 1 (ON, True), mueve el bit más significativo MBS a la posición del bit menos significativo LSB y viceversa, y así sucesivamente con los demás bits.  
```
    MSB      bits       LSB
    n n-1 n-2 ... 2  1  0
    ^  ^   ^      ^  ^  ^
    |  |   |      |  |  |
    |  |   +------+  |  |
    |  +-------------+  |
    +-------------------+
```

> Intercambios en los bits:  

MSB <-> 0  
MSB-1 <-> 1  
MSB-2 <-> 2  


Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  

---

### INTERCAMBIO DE BYTES Y TETRADAS  
```
       {dest:=}  
    --{SWAP src}--  
```
Si la señal en la entrada lógica de la instrucción es 1 (ON, True), esta instrucción intercambia bytes y tétradas (nibbles, cuartetos) de una variable.

En función del tamaño en bytes de la variable (alcance, span), el resultado es diferente:  

1 byte: intercambia las tétradas dentro de un int8 (BYTE), p. ej. 0x73 -> 0x37  
```
    MSB       bits       LSB
    7  6  5  4  3  2  1  0
    \________/  \________/
         ^           ^
         |           |
         +-----------+
```

> Intercambios en los bits:  

7 <-> 3  
6 <-> 2  
5 <-> 1  
4 <-> 0  

2 bytes: intercambia los bytes dentro de un int16 (WORD), p. ej. 0x7733 -> 0x3377  
```
    MSB                  bits                   LSB
    15 14 13 12 11 10 9 8  7  6  5  4  3  2  1  0
    \___________________/  \____________________/
                    ^           ^
                    |           |
                    +-----------+
```
>Intercambios en los bits:  
15 <-> 7  
14 <-> 6  
...  
9 <-> 1  
8 <-> 0  

3 bytes: intercambia las tétradas del byte central, y también ambos bytes de los extremos de un int24, p. ej,. 0x775A33 -> 0x33A577
```
    MSB                             bits                          LSB
     23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8 7 6 5 4 3 2 1 0
     \_____________________/ \_________/ \_________/ \_____________/
                    ^             ^           ^             ^
                    |             |           |             |
                    |             +-----------+             |
                    +---------------------------------------+
```
>Intercambios en los bits:  
15 <-> 7  
23 <->  7  
...  
16 <->  0  
15 <-> 11  
...  
12 <->  8  

4 bytes: intercambia los bytes de los extremos (3 y 0) y también los bytes centrales (2 y 1) en un int32 (DWORD), p. ej. 0x7755AA33 -> 0x33AA5577  
```

  MSB                                          bits                                  LSB
  31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
  \_____________________/ \_____________________/ \___________________/ \_____________/
                 ^                        ^           ^                       ^
                 |                        |           |                       |
                 |                        +-----------+                       |
                 +------------------------------------------------------------+
```
>Intercambios en los bits:  
15 <-> 7  
31 <->  7  
...  
24 <->  0  
23 <-> 15  
...  
16 <->  8  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  

---

### REGISTRO DE DESPLAZAMIENTO CON VARIABLES  
```
     {SHIFT REG}  
    --{var0..3}--  
```
Un registro de desplazamiento con variables está asociado con un conjunto de variables "var (índice)".  

La entrada de datos al registro de desplazamiento es la variable con índice 0 (var0).  
En cada flanco positivo (ascendente) en la entrada lógica de la instrucción, se desplaza el valor de la variable en cada registro i a la variable en el registro a su derecha (i + 1).  
El valor de la variable en el registro de mayor índice es descartado.  

Por ejemplo, el registro de desplazamiento {SHIFT REG} --{reg0..3}--se asocia con las variables reg0, reg1, reg2 y reg3.  

Ésto significa que al activarse la instrucción, se asigna reg3: = reg2, luego reg2: = reg1 y finalmente reg1: = reg0.  

reg0 se mantiene inalterado al ejecutar la instrucción; luego de ejecutarla se puede mover un nuevo valor a la variable de entrada (reg0), el que posteriormente será desplazado a las variables subsecuentes del registro en cada nueva operación de desplazamiento.  

> **NOTA:** un Registro de Desplazamiento con gran número de variables puede consumir fácilmente una gran cantidad de memoria.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### BUSQUEDA EN TABLA  
```
      {dest := }  
    --{ LUT[i] }--  
```
Una tabla de consulta es un conjunto ordenado de n pares de valores i <-> value.  

Si la señal en entrada lógica de la instrucción es 1 (ON, True), la variable "dest" tendrá un valor igual a la entrada correspondiente al índice i en la tabla de búsqueda.  

El índice comienza desde cero, de modo que el índice i debe ser un número entero entre 0 y n - 1.  
El comportamiento de esta instrucción es indefinido si el índice está fuera de este rango.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), la variable "dest" no modifica su valor.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### LINEARIZACION POR TRAMOS  
```
        {yvar := }  
    --{ PWL[xvar] }--  
```
Esta instrucción permite aproximar una función complicada o curva.  

Por ejemplo, podría ser útil si está intentando aplicar una curva de calibración para convertir una salida no lineal de un sensor a unidades más convenientes.  

Suponga que está intentando aproximar una función que convierte una variable de entrada entera, x, a una variable de salida entera, y desea conocer el valor se la función en diferentes puntos.  

Por ejemplo, usted puede saber que la salida de:  

```
f(0)   = 2  
f(5)   = 10  
f(10)  = 50  
f(100) = 100  
```

Ésto significa que los puntos forman una curva:

```
(x0, y0)   = (  0,   2)  
(x1, y1)   = (  5,  10)  
(x2, y2)   = ( 10,  50)  
(x3, y3)   = (100, 100)  
```

Usted puede introducir esos cuatro puntos en una tabla asociada con la instrucción Linearización por Tramos.  
La instrucción leerá el valor de la variable "xvar" y calculará el valor de la variable "yvar" de  modo tal que la curva aproximada mediante tramos lineales pase por todos los puntos de referencia; por ejemplo, si establece xvar = 10, entonces la instrucción establecerá yvar = 50.  

Si a la instrucción se le ingresa un valor de xvar que está entre dos de los valores de x para los cuales se le ha fijado puntos en la tabla asociada, entonces la instrucción establecerá yvar de modo que (xvar, yvar) se encuentre en la línea recta que une esos dos puntos en la tabla.  

En el ejemplo, xvar = 55 dará una salida de yvar = 75: los dos puntos de la tabla son (10, 50) y (100, 100). 55 está a medio camino entre 10 y 100, y 75 está a medio camino entre 50 y 100, por lo que (55, 75) se encuentra sobre la línea que une esos dos puntos.  

Los puntos deben ser especificados en orden ascendente por sus coordenadas x. 

Los valores de los pares de puntos x-y ingresados eventualmente podrían impedir realizar las operaciones aritméticas necesarias para ciertas tablas de consulta usando variables enteras de 16 bits; si este es el caso, entonces LDmicro mostrará una advertencia.  

Por ejemplo, esta tabla de búsqueda producirá un error:  

```
(x0, y0)    = (  0,   0)  
(x1, y1)    = (300, 300)  
```

Puede corregir este tipo de errores haciendo más pequeña la distancia entre puntos de referencia.  
Por ejemplo, esta tabla es equivalente a la de arriba, y no produce un error:  

```
(x0, y0)    = (  0,   0)  
(x1, y1)    = (150, 150)  
(x2, y2)    = (300, 300)  
```
Casi nunca será necesario utilizar más de cinco o seis puntos. Añadir más puntos hace que su código resulte más grande y más lento de ejecutar.  

Si se pasa un valor a xvar mayor que la mayor coordenada x, o menor que la menor coordenada x, establecidas en la tabla de referencia, el comportamiento de la instrucción resulta indefinido.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### CIRCUITO CERRADO, CIRCUITO ABIERTO  
```  
    --+---+--    --+   +--  
```
La condición de salida de un circuito cerrado es siempre igual a su condición de entrada. La condición de salida de un circuito abierto es siempre 0 (OFF, False). Éstas instrucciones son útiles para realizar depuración en el programa de usuario.  

---

### RELE DE CONTROL MAESTRO  
```
    --{MASTER RLY}--  
```
De forma predeterminada, la condición de inicio de cada escalón (rung) es verdadera.  

Si una instrucción de Relé de Control Maestro se ejecuta con una señal de 0 (OFF, False) provista por una instrucción previa en su entrada lógica, entonces la condición de inicio para todos los escalones (rungs) subsiguientes será falsa.  

Ésto persistirá hasta que se alcance la siguiente instrucción de Relé de Control Maestro (con independencia de la condición de entrada de dicha instrucción), desactivando por lo tanto la ejecución de toda la lógica programada en los escalones (rungs) intermedios entre ambas instrucciones MASTER RLY.  

Por consiguiente, esta instrucción debe utilizarse por pares: la primera para iniciar la sección de programa que eventualmente requiera ser deshabilitada en función de determinadas condiciones, y la segunda para finalizar dicha sección.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### GOTO  
```
         ?  
    --{GOTO}--  
```
La instrucción GOTO() permite realizar saltos en el orden de ejecución del programa, en función de la salida de otra instrucción previa que provea la señal a su entrada lógica.  

La instrucción GOTO() puede ser utilizada de dos modos: a) con un argumento numérico "n", b) con un argumento textual "name".

a) La instrucción GOTO(n), en la que "n" es un número entero, causa un salto en la ejecución del programa al escalón (rung) identificado con el número n:  

* Si n es igual a 0 (n==0) la ejecución del programa salta al comienzo del ciclo de PLC: se comprueba el temporizador de ciclo (PLC timer), se restablece el temporizador de control (CLRWDT), y se comienza a ejecutar el programa de usuario desde su primera instrucción.  
* Si n es menor que 0 (n<0) la ejecución del programa salta a la dirección 0 de la memoria de programa: se produce un restablecimiento por software (SOFT RESET) del microcontrolador.
* Si n es mayor que el número de escalones en el esquema Ladder (n>max_rung), la ejecución del programa salta todas las instrucciones subsiguientes y recomienza el ciclo de PLC.  

b) La instrucción GOTO(name), en la que "name" es el nombre asignado a la etiqueta en una instruccion LABEL(name), causa un salto en la ejecución del programa al escalón (rung) en el que se ha programado la correspondiente instrucción de destino LABEL(name).  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### LABEL  
```
         ?  
    --{LABEL}--  
```

La instrucción LABEL(name) identifica el punto de destino para el salto en la ejecución del programa, generado por una instrucción GOTO(name).  

Los nombres asignados a cada una de las instrucciones LABEL(name) y a su correspondiente llamada GOTO(name) deben ser únicos en cada programa, distinguen mayúsculas y minúsculas, y no deben comenzar con un número.  

---
### SUBPROG, ENDSUB, GOSUB, RETURN  

Este conjunto de instrucciones permite gestionar llamadas a subprogramas (subrutinas) que se pueden procesar una o más veces en el transcurso de un mismo ciclo de PLC.  

Las instrucciones que se procesarán durante la ejecución de un subprograma (subrutina) deberán ser programadas en escalones (rungs) ubicados entre el correspondiente par de instrucciones SUBPROG(name) y ENDSUB(name).  

Luego de finalizar el procesamiento de un subprograma SUBPROG(name), ya sea por haber alcanzado la instrucción ENDSUB(name) o por haber alcanzado una instrucción RETURN, la ejecución del programa principal continuará por la instrucción subsiguiente a la instrucción GOSUB(name) que ha iniciado la ejecución de la correspondiente instrucción SUBPROG(name).  
```
          ?  
    --{SUBPROG}--  
```
La instrucción SUBPROG(name) señaliza el inicio del subprograma (subrutina) que se procesará al ejecutarse la correspondiente instrucción de llamada GOSUB(name), y obligatoriamente debe ser finalizada con una instrucción ENDSUB(name).  
```
          ?  
    --{ENDSUB}--  
```
La instrucción ENDSUB(name) señaliza la finalización del subprograma (subrutina) que se procesará luego de una instrucción de llamada a subprograma GOSUB(name).  
```
         ?  
    --{GOSUB}--  
```
La instrucción GOSUB(name) habilita la llamada para el procesamiento de la correspondiente subrutina SUBPROG(name); puede implementarse como incondicional, al alcanzarse determinados escalones durante la ejecución del programa; o como condicional en función de la señal en su entrada lógica, provista por una instrucción previa.  

Una misma instrucción GOSUB (name) puede ser programada en diferentes escalones (rungs) del programa principal, tanto en forma condicional como incondicional.  

Si la señal en entrada lógica de la instrucción es 1 (ON, True), se procesará el correspondiente subprograma señalizado por la instrucción SUBPROG(name).  

Si la señal en entrada lógica de la instrucción es 0 (OFF, False), no se procesará la correspondiente instrucción SUBPROG(name) y la ejecución del programa continuará inmediatamente por la instrucción subsiguiente a la instrucción de llamada GOSUB(name).  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  
```
    --{RETURN}--  
```
La instrucción RETURN permite interrumpir el procesamiento del subprograma SUBPROG(name) sin necesidad de completar su ejecución, o sea sin necesidad de alcanzar la correspondiente instrucción ENDSUB(name).  
Debe utilizarse siempre en forma condicional, es decir en función de la señal en su entrada lógica, provista por una instrucción previa.  

La instrucción RETURN puede ser programada en diferentes escalones (rungs) del subprograma, entre las correspondientes instrucciones SUBPROG(name) y ENDSUB(name), permitiendo diferentes condiciones de finalización del procesamiento del subprograma.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

Ver:  
[GOTO and LABEL instruction](https://github.com/LDmicro/LDmicro/wiki/GOTO-instruction)  
[SUBPROG, RETURN, ENDSUB and GOSUB, LABEL and GOTO instruction. Part 2](https://github.com/LDmicro/LDmicro/wiki/SUBPROG,-RETURN,-ENDSUB-and-GOSUB,-LABEL-and-GOTO-instruction.-Part-2)  
[SUBPROG, RETURN, ENDSUB and GOSUB, LABEL and GOTO instruction. Part 3](https://github.com/LDmicro/LDmicro/wiki/SUBPROG,-RETURN,-ENDSUB-and-GOSUB,-LABEL-and-GOTO-instruction.-Part-3)  

---

### RESTABLECER TEMPORIZADOR DE CONTROL (WATCHDOG)  
```
    --{CLRWDT}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), entonces se restablece (se pone a 0) la variable del temporizador WDT del microcontrolador.  

LDmicro ejecuta el comando CLRWDT automáticamente al comienzo de cada ciclo de PLC, por lo que puede utilizarse un CLRWDT adicional si el período del ciclo del PLC es mayor que el período del temporizador y WDT ha sido previamente habilitado modificando los bits de configuración del microcontrolador.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### BLOQUEAR EJECUCION
```  
    --{LOCK}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), se ejecutará el comando LOCK.  

El comando LOCK es un método para llevar a un punto muerto la ejecución del programa y está diseñado como un bucle cerrado. El comando LOCK ejecuta GOTO a la dirección actual, ej. labelN: GOTO labelN (label02e7: rjmp label02e7), es decir, resulta en un bucle infinito.  

El microcontrolador ejecutará dicho bucle infinito, pero si el WDT ha sido previamente habilitado modificando los bits de configuración del microcontrolador, puede restablecer la ejecución del programa.  

Sólo el WDT o un restablecimiento externo (mediante hard reset por !MCLR, o por corte y restablecimiento de la alimentación del MCU) pueden desbloquear el programa después de ejecutado el comando LOCK.  

---

### SUSPENDER  
```
    --{SLEEP}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), se establecerá el modo SLEEP (suspendido, modo de bajo consumo).  

Puede utilizarse para ahorrar energía cuando la alimentación del microcontrolador depende del suministro de una batería.  

* PIC: puede salir del modo de suspensión mediante una interrupción externa de flanco ascendente en el pin RB0 / INT.  
* PIC10xxxx: puede salir del modo de suspensión mediante un cambio de estado en los pines GP0, GP1, GP3.  
* AVR: puede salir del modo de suspensión mediante interrupción externa de flanco ascendente en los pines PD2 / INT0, PD3 / INT1.  

La operación SLEEP no afecta a otros pines de Entrada/Salida del microcontrolador ni a otras operaciones de LDmicro.  

> **Nota:**  
> La ejecución de la instrucción SLEEP alarga el tiempo de ciclo del PLC y la ejecución de los temporizadores TON, TOF, RTO, TCY. Ésto puede romper el flujo normal de trabajo del programa, provocando un error en la aplicación.  

Esta instrucción siempre debe ser programada en el extremo derecho del escalón (rung).  

---

### GENERADOR ALEATORIO  
```
        var  
    --{ RAND }--  
```
El generador de números pseudo-aleatorios devuelve un número al azar dentro del rango completo de la variable "var".  

Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), la variable destino "var" tendrá el siguiente número pseudo-aleatorio calculado por el generador congruencial lineal (LCG). 

Si la señal en la entrada lógica de esta instrucción es 0 (OFF, False), entonces nada sucede.  

 El generador se define por la relación de recurrencia:  

```
    X[n+1] = (a * X[n] + c) mod m  
```
 
Constantes usadas desde el VMS MTH $ RANDOM, versiones antiguas de glibc:  
```
    a = 69069 ( 0x10DCD )  
    c = 1 (0x01)  
    m = 2^32  (0x100000000)  
    X = (X * 0x10DCD + 0x01) % 0x100000000  
```    
RAND devuelve los bytes más significativos de X.  
X almacenado como $seed_Rand ( variable de 32 bits).  

Ver [Linear_congruential_generator](https://en.m.wikipedia.org/wiki/Linear_congruential_generator)  

---

### INICIALIZAR GENERADOR ALEATORIO  
```
      {SRAND     Rand}  
    --{$seed:=newSeed}--  
```
El generador de números pseudo-aleatorios se inicializa utilizando el argumento pasado como newSeed (semilla).  

Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), la variable destino $seed_Rand será igual a la variable de origen o la constante.  

Si la señal en la entrada lógica de esta instrucción es 0 (OFF, False), entonces nada sucede.  
Dos inicializaciones diferentes con la misma semilla generarán la misma sucesión de resultados en llamadas posteriores a RAND.  

Si la semilla se pone a 1, el generador 'Rand' se reinicializa a su valor inicial y produce los mismos valores que antes de cualquier llamada a RAND o SRAND.  

Las fuentes de entropía para la generación de la variable newSeed (semilla) pueden ser lecturas de la variable de un ADC (especialmente si el pin de entrada no está conectado a circuito alguno y recibe inducción por fuentes externas), temporizadores, valores de RAND anteriores guardados en EEPROM, etc.  

---

### GENERADOR DE CARACTERES EN LED DE 7 SEGMENTOS  
```
      {7SEG    dest:=}  
    --{C          src}--  
```

Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), la variable destino "dest" será un valor binario abcdefgP que controlará la activación de los segmentos y el punto decimal del display LED de 7 segmentos.  

La variable "src" debe estar en el rango 0...127 ó 0...128, y usualmente puede tener un tamaño de 2 bytes, o de 1 byte si no utilizará el carácter ° (grado), código ASCII 128.  

Utilice el parámetro A ó C de la instrucción para especificar cuál tipo de conexión común (Ánodo o Cátodo) será utilizada.  

Se puede asignar un variable de acceso directo "#PORTx" como variable "dest", de modo de controlar en forma directa un display conectado a los pines del puerto correspondiente, sin necesidad de utilizar instrucciones intermedias.  

Los pines de salida de dicho puerto podrían conectarse a directamente los ánodos (o cátodos) del display LED de 7 segmentos, a través de resistores de limitación de corriente.  

> P. ej. para encender el segmento de LED "a":  
1. cuando es utilizado un módulo con ánodo común, se requiere un nivel bajo en el pin "a" y un nivel alto en el pin común.  
2. cuando es utilizado un módulo con cátodo común, se requiere un nivel alto en el pin "a" y un nivel bajo en el pin común.  

**Ver NOTA abajo.**  

Display de LED de 7 segmentos: identificación de cada segmento.  
```
     ___
   f| a |b
    |___| 
    | g |
   e|___|c
      d  ·P

  |SEGMENTO   |P|g|f|e|d|c|b|a|  
  |"dest" BIT |7|6|5|4|3|2|1|0|  
```
Están implementados 129 caracteres de la tabla de códigos ASCII.  
Los primeros 32 caracteres ASCII son reemplazados por los dígitos hexadecimales correspondientes 00 a FF.  

El 129° código corresponde al símbolo ° (grados), utilizado habitualmente para mostrar un valor de temperatura o ángulo.  

```
  |ADDRESS  |   Low nibble   |   High nibble  |  
  |  0(0x00)|0123456789ABCDEF 0123456789ABCDEF|  
  | 32(0x20)| !"#$%&'()*+,-./ 0123456789:;<=>?|  
  | 64(0x40)|@ABCDEFGHIJKLMNO PQRSTUVWXYZ[\]^_|  
  | 96(0x60)|`abcdefghijklmno pqrstuvwxyz{|}~ |  
  |128(0x80)|° (degree, 0x80)                 |

Carácter " ", espacio (en blanco): hex 0x20  
Carácter DEL, eliminar (en blanco): hex 0x7F  
```
Ver [X-segment LED display, font example](https://github.com/LDmicro/LDmicro/wiki/X-segment-LED-display,-font-example)  

**NOTA DE APLICACION EN OSIMPLC:**  
La conexión directa del display de 7 segmentos a los transistores de  los optoacopladores de salida no debería realizarse con OSIMPLC. Es preferible utilizar módulos de salida a transistor de potencia O4T conectados a Y0-Y3 e Y4-Y7 (Y0=bit0; Y7=bit7): cada una de sus salidas admite una corriente máxima de 2A y está protegida por fusible y diodo de rueda libre y máxima tensión, permitiendo utilizar displays de LED de 7 segmentos de gran potencia lumínica.  

Las salidas Y8-YB podrían ser utilizadas para multiplexar las salidas de la instrucción 7SEG y controlar hasta 4 displays de 7 segmentos (4 dígitos, o 3 dígitos y signo).  

Recuerde que en OSIMPLC las salidas de transistor de potencia son MOSFET tipo N, la conexión  eléctrica del LED 7 segmentos debe ser NPN, sink (ánodo común).  

En esta implementación, el parámetro de conexión común de la instrucción debe establecerse en C, de modo que la salida lógica para activar cada segmento resulte un 1, y un 0 para desactivarlo (al contrario de la conexión eléctrica).  

---

### CONVERSOR BINARIO A BCD  
```
  {BIN2BCD dest:=}  
    --{src}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), la instrucción convierte el valor binario 0bn de la variable "src" en un código BCD y lo transfiere a la variable "dest".  

"src" puede ser un valor binario con tamaño 1, 2, 3 o 4 bytes.  

"dest" es un valor BCD "desempaquetado", con tamaño 3, 5, 8 ó 10 bytes.  

"Desempaquetado" significa que cada numeral (dígito decimal) es codificado en un byte, donde los cuatro bits inferiores representan el numeral, y los cuatro bits superiores no tienen significancia.  

> Por ejemplo:  
> El número decimal 99 (0x63) será convertido en código BCD en notación hexadecimal 0x000909.  
> El número decimal 100 (0x64) será convertido en código BCD en notación hexadecimal 0x010000.  


Ver [Binary-coded_decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal)  

---

### CONVERSOR BCD A BINARIO  
```
  {BCD2BIN dest:=}  
    --{src}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), la instrucción convierte el código BCD de la variable "src" en un valor binario y lo transfiere a la variable "dest".  

---

### CODIFICADOR DE CUADRATURA (QUAD ENCODER)
```  
      ~~[XqA0      XqB0       qDir0]-  
    --\[XqZ0   QUAD ENCOD   qCount0]^--  
```
La instrucción Codificador de Cuadratura toma dos señales cuadradas desplazadas en 90° (señales A y B codificadas en cuadratura) y una tercera señal de reposición (señal Z) desde un dispositivo encoder incremental rotativo o lineal.  
Provee como salidas la variable del contador interno de la instrucción, un pulso positivo en su salida lógica cuando hay un cambio en dicha variable, y una salida de señal en un relé interno R ó un pin Y del microcontrolador, para indicar la dirección de conteo (1 = ascendente, 0 = descendente).  
```
                        move ->             |            <- move
         ^
         | ___         _______         ___________          _______         ___
  Input A|    |_______|       |_______|           |________|       |_______|
         |
         | _______         _______         ___         ________         _______
  Input B|        |_______|       |_______|   |_______|        |_______|
         |
         |
  Counter|  0 |   | 1 |   | 2 |   | 3 |   | 4 |   | 3 |   | 2 |   | 1 |   | 0
         |
   Output| ___________________________________   decrease or CCW or backward
      Dir|    increase or CW or forward       |________________________________
         |
   Output|
    Pulse| _______^_______^_______^_______^_______^_______^_______^_______^____
         |
       --+----------------------------------------------------------------> time
         |
           '^' - es un pulso con duración de 1 ciclo de PLC
```

Ver [Incremental_encoder](https://en.wikipedia.org/wiki/Incremental_encoder)  

La instrucción puede ser implementada con sólo dos entradas de señal (señal A -> XqAn y señal B->  XqBn), o bien con tres entradas de señal (utilizando también señal opcional Z -> XqZn), provenientes de los pines de entrada del microcontrolador.  

La salida de señal opcional Dir (Dirección -> YqDirn) puede ser dirigida a un pin de salida del microcontrolador, o ser omitida.  

Las entradas lógicas para las señales A y B obligatoriamente deben asignarse a pines de entrada pertenecientes a un mismo puerto del microcontrolador (p. ej. RD0 y RD1).  

La frecuencia de los pulsos en las entradas A y B debe ser menor a una cuarta parte de la frecuencia del ciclo de PLC (f[A,B] < fPLC/4), de lo contrario se perderán lecturas de pulsos, generando errores de conteo.  

Los rebotes de señal (bounces) que pudieran producirse en los pulsos de entrada en las señales A y B deben tener una duración menor a un octavo de ciclo del PLC (Tbounce[A,B] < Tcycle/8), de lo contrario  se producirán lecturas repetitivas de un mismo pulso, generando errores de conteo.  

La entrada lógica para la señal Z es opcional y puede dejarse vacía (borrando el nombre de la variable asignada por defecto en la instrucción), sin asignarla a pin de entrada en el microcontrolador.  

La salida lógica para la señal Dir es opcional y puede dejarse vacía (borrando el nombre de la variable asignada por defecto en la instrucción), sin asignarla a relé interno o pin de salida en el microcontrolador.  

Si la señal en la entrada lógica de la instrucción es 1 (ON, True), entonces los pulsos en las entradas A, B y Z son decodificados y asignados al valor de la variable del contador interno de la instrucción (qCount0 por defecto).  

La instrucción Codificador de Cuadratura ejecuta conteo doble, leyendo las transiciones (flancos positivos y negativos) que aparecen en la señal B y evaluando simultáneamente el nivel (0, 1) de la señal A.  

Cuando un flanco en la señal lógica en la entrada B genera un conteo, se modificará la variable del contador interno qCount0 (incrementando o decrementando), se generará un pulso con duración de un ciclo de PLC en la salida lógica de la instrucción, y la señal en la salida opcional Dir será 1 (ON, True) si el contador es incrementado, y 0 (OFF, False) si es decrementado.  

El valor actual de la variable interna de conteo qCount0 puede leerse como cualquier otra variable de contador, utilizando la instrucción MOV, y luego ejecutar sobre la variable destino las operaciones aritméticas y de comparación necesarias para la aplicación.  

No es recomendado utilizar la instrucción MOV para asignar un valor a la variable del contador interno de la instrucción, a menos que resulte imprescindible para la aplicación. 

 Para resolver pequeños errores en el conteo (pérdida o sobrelectura de algunos pulsos) se puede utilizar la entrada Z (ver abajo).  
 
**Consejo:** Utilice la salida lógica de pulsos de la instrucción para determinar si la variable del contador interno se ha modificado, no para efectuar el conteo (puede leer su valor actual desde la variable del contador interno de la instrucción).  

La evaluación de la señal lógica en la entrada Z (reposición) puede ser dinámica (activada por flanco positivo o negativo), o estática (activada por nivel alto o bajo).  

Si el parámetro Count per revol de la instrucción es 0, al activarse la señal Z la variable del contador interno qCount0 será automáticamente restablecida a 0 (reset).  

Si el parámetro Count per revol de la instrucción es mayor que 0, al activarse la señal Z la variable del contador interno qCount0 será automáticamente establecida al siguiente múltiplo del parámetro Count per rev (mayor si la variable estaba incrementando, y menor si la variable estaba decrementando).  
Si el parámetro Count per revol de la instrucción es menor que 0, las señales en la entrada Z son ignoradas.  

Si la señal en la entrada lógica de la instrucción es 0 (OFF, False), entonces las señales en las entradas A, B y Z son ignoradas (no se procesan).  

> *NOTA:*
> Para simular las señales lógicas en las entradas A, B y Z, debe hacer doble click sobre su nombre en la lista de variables en la zona inferior de la ventana de LDmicro.  

Vea detalles en:
[Incremental-QUADRATURE-ENCODER](https://github.com/LDmicro/LDmicro/wiki/Incremental-QUADRATURE-ENCODER)  
[Incremental QUADRATURE ENCODER controls the brightness of the LED (PWM out)](https://github.com/LDmicro/LDmicro/wiki/Incremental-QUADRATURE-ENCODER-controls-the-brightness-of-the-LED-(PWM-out))

---

### OPERACIONES DE COMUNICACION SERIAL DE DATOS POR UART (RECEPCION Y TRANSMISION)

LDmicro puede generar código para utilizar el periférico UART incorporado en ciertos modelos de microcontroladores PIC y AVR.  

En AVRs con múltiples UARTs, solamente UART1 (no UART0) puede ser implementado. Configure la velocidad de comunicación (baudrate) en baudios utilizando el menú Configuraciones -> MCU parámetros...  

Eventualmente, no podrán obtenerse ciertas velocidades de comunicación en baudios, ya que se establecen en función de la frecuencia base del microcontrolador; LDmicro mostrará una advertencia si éste es el caso.  

En LDmicro existen diferentes instrucciones disponibles para enviar caracteres individuales, bytes en modo binario ("crudo") o cadenas de caracteres formateadas que incluyen un texto fijo, el valor de una variable y caracteres de control.  

> **NOTA:**  
> **Es imprescindible asegurar que el buffer de salida de la UART está libre antes de tratar de enviar un nuevo carácter, byte en modo binario o cadena formateada.**  

Para cumplir esta condición, usted puede utilizar diferentes estilos de programación:  

1. Utilice un tiempo de ciclo de PLC que resulte considerablemente mayor que el tiempo requerido para transmitir un solo carácter o byte en el baudrate deseado. P. ej., si configura el tiempo de ciclo en 10 ms (por defecto) y efectuará la comunicación utilizando un baudrate de 9600 bits por segundo, el tiempo de ciclo será aproximadamente 10 veces mayor al tiempo de transmisión de cada carácter.  
2. Utilice un temporizador para insertar un retardo entre caracteres o bytes durante la transmisión. El parámetro del temporizador debe ser mayor que el tiempo de transmisión del carácter o byte en la UART.  
3. Compruebe que el estado de la salida lógica de la instrucción de envío es 0 (OFF, False) para asegurar que el carácter o byte previo ya ha sido transmitido, antes de tratar de enviar un nuevo carácter o byte ; p. ej. envíe la salida de la instrucción UART SEND o UART SENDn a una bobina de un relé interno Rname, y utilice su contacto normal abierto como condición de entrada de la misma instrucción.  
4. Utilice la instrucción UART SEND: está listo? como condición de entrada de la instrucción UART SEND, UART SENDn o ENVIAR CADENA FORMATEADA POR UART.  

---

### RECIBIR CARACTER POR UART  
```
          var  
    --{UART RECV}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), UART RCV intenta recibir un solo carácter desde el buffer de entrada de la UART.  

Si efectivamente se lee un carácter, su valor ASCII se almacena en la variable "var"y la salida de la instrucción será 1 (ON, True) para un solo ciclo de escaneo de programa en el PLC.  
Si no se lee ningún carácter, entonces la salida de la instrucción será 0 (OFF, False).  
Si la señal en la entrada lógica de esta instrucción es 0 (OFF, False), entonces nada sucede.  

---

### ENVIAR CARACTER POR UART  
```
          var  
    --{UART SEND}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), entonces UART SEND escribirá un solo carácter en el buffer de salida de la UART. El valor ASCII del carácter debe ser previamente almacenado en "var".  

La salida de la instrucción es 1 (ON, True) si el UART está ocupado (transmitiendo actualmente un carácter), y 0 (OFF, False) en caso contrario.

Si la señal en la entrada lógica de esta instrucción es 0 (OFF, False), entonces nada sucede.  

Tenga en consideración que los caracteres tardan algún tiempo en transmitirse. Sólo se debe  activar la entrada lógica de la instrucción (intentar enviar un carácter) cuando el buffer de transmisión del UART no está ocupado.  

> **NOTA:**  
> Lea más abajo respecto al uso de la instrucción ENVIAR CADENA FORMATEADA POR UART, antes de usar la instrucción UART SEND.  

La instrucción para enviar una cadena formateada de caracteres es mucho más fácil de usar, y casi con toda seguridad es capaz de realizar la tarea de comunicación de datos que usted desea.

---

### Actualización: Versiones >= v.4.4.0  

**Las instrucciones UART RCV y UART SEND permiten recibir/enviar variables de tamaño mayor a un solo byte.**  

El parámetro "Number of bytes to receive/send" debe ser igual o menor que el tamaño de la variable utilizada.  

El valor 1 es compatible con versiones anteriores de LDmicro.  

El parámetro "Wait until all bytes are received/transmitted:" controla el algoritmo de recepción/transmisión: si es igual a 1, todos los bytes son recibidos/transmitidos en un solo paquete; si es igual a 0, sólo se recibe/transmite un byte por ciclo de escaneo del PLC.  
El valor 0 es compatible con versiones anteriores de LDmicro.  

Cuando todos los bytes han sido recibidos/transmitidos, la salida de la instrucción será 1 (ON, True) durante un ciclo del PLC.  

Ver [UART communication FAQ part 2](https://github.com/LDmicro/LDmicro/wiki/UART-communication-FAQ-part-2)  

---

### ENVIAR BYTE EN MODO BINARIO POR UART  
```
          var  
    --{UART SENDn}--  
```
Si la señal en la entrada lógica de esta instrucción es 1 (ON, True), entonces UART SENDn escribirá una variable de un byte en formato binario ("crudo") en el buffer de salida de la UART.  
Se envía un solo byte por cada ciclo de PLC, al igual que en la instrucción ENVIAR CADENA FORMATEADA POR UART.  

La salida de la instrucción es 1 (ON, True) si el UART está ocupado (transmitiendo actualmente el byte en modo binario), y 0 (OFF, False) en caso contrario.  
Si la señal en la entrada lógica de esta instrucción es 0 (OFF, False), entonces nada sucede.  

---

### ENVIAR POR UART: está listo?  
```
       Is ready?  
    --[UART SEND]--  
```
La señal de entrada lógica de la instrucción no se evalúa (no se tiene en cuenta).  
La salida es 1 (ON, True) cuando el buffer de transmisión de la UART está vacío y listo para transmitir nuevos datos, y 0 (OFF, False) en caso contrario.  

---

### RECIBIR POR UART: hay dato disponible?  
```
       Is avail?  
    --[UART RECV]--  
```
La señal de entrada lógica de la instrucción no se evalúa (no se tiene en cuenta).  
La salida es 1 (ON, True) cuando hay datos no leídos en el buffer de recepción, y 0 (OFF, False) en caso contrario.  

---

### ENVIAR CADENA FORMATEADA POR UART  
```
             var  
    --{"value: \c\r\n"}--  
```
Cuando aparece un flanco positivo de la señal en la entrada lógica de esta instrucción (transición de 0 (OFF, False) a 1 (ON, True), se comienza a enviar una cadena completa de caracteres a través del puerto serie UART.  

El texto "value:" puede ser reemplazado por cualquier texto fijo en función de los requerimientos de la aplicación.  

Si la cadena contiene la secuencia especial \\c, entonces esa secuencia será sustituída por el valor de la variable "var", automáticamente convertido en una cadena de caracteres.  

La variable "var" se formateará para ocupar exactamente c caracteres, por lo tanto c debe ser un número igual o mayor que la cantidad de caracteres que expresan la variable numérica en decimal. 

Por defecto, al programarse la instrucción en el esquema Ladder, LDmicro configura \\c en el valor 3 e incluye los caracteres de control \\r y \\n que generan los códigos ASCII para retroceso de carro y nueva línea, respectivamente.  

Por ejemplo, si se reemplaza "value:" por "Temp:", \\c es igual a 3, la variable "var" contiene un valor igual a 35 y se conservan los caracteres de control por defecto, entonces la cadena transmitida será "Temp:&nbsp;&nbsp;35" (note el espacio en blanco extra antes del valor de la variable), e incluirá automáticamente los comandos para retroceso de carro y nueva línea en el terminal serial.  

Si en cambio "var" fuera igual a 1432, entonces el comportamiento resultará indefinido, porque 1432 tiene más de tres dígitos. En ese caso sería necesario utilizar \\c igual a 4 para no generar un error.  

Si la variable puede ser negativa, utilice \\-3 o \\-4, etc. en su lugar. Esto hará que LDmicro imprima un espacio en blanco inicial "&nbsp;" al transmitir números positivos, y un signo menos "-" inicial al transmitir números negativos.  

También puede utilizar valores haxadecimales para conformar la cadena a enviar, p. ej. para enviar caracteres de control, o de la codificación ASCII extendida (rango de 0x80 a 0xFF).  

Es posible usar esta instrucción para dar enviar sólo texto, es decir una cadena fija de caracteres por UART sin interpolar el valor de una variable entera en el texto que se envía. En este caso, simplemente no incluya en la instrucción la secuencia de escape \\c correspondiente al número de caracteres de la variable. Sí puede incluir opcionalmente caracteres de control \\r, \\n y otros.  
Utilice "\\\" si necesita escapar y enviar una barra invertida literal incluída en el texto fijo.  

Además de la secuencia de escape \\c para interpolar una variable, están disponibles las siguientes secuencias de escape:  

```
  | Sec |  Hex |        ASCII        | 
  | \a  | 0x07 | BEL, Alert (Bell)   |
  | \b  | 0x08 | BS, Backspace       |
  | \e  | 0x1B | Escape character    |
  | \f  | 0x0C | FF, Formfeed        |
  | \n  | 0x0A | NL, Newline         |
  | \r  | 0x0D | CR, Carriage Return |
  | \t  | 0x09 | TAB, Horizontal Tab |
  | \v  | 0x0B | VT, Vertical Tab    |
  | \'  | 0x27 |Single quotation mark|
  | \"  | 0x22 |Double quotation mark|
  | \?  | 0x3F |Question mark        |
  | \\  | 0x5C |Backslash            |
  | \xhh|(byte)|hh range: 0x00..0xFF |  
```

La salida de esta instrucción es 1 (ON, True) mientras está transmitiendo datos, y 0 (OFF, False) en caso contrario.  

Si se activan simultáneamente múltiples instrucciones para enviar cadenas formateadas (o si una es energizada antes de que otra similar termine de ejecutarse), o si estas instrucciones son activadas simultáneamente con otros tipos de instrucciones para transmisión de datos por UART, entonces el comportamiento resultará indefinido.  

**NOTA IMPORTANTE:**  

Esta instrucción consume gran cantidad de memoria del programa, por lo que debe utilizarse con moderación. La implementación actual no es eficiente, pero para una mejor solución sería necesario realizar modificaciones en todos los back-ends.  

---

### OPERACIONES DE COMUNICACION DE DATOS POR BUSES I2C Y SPI, Y FORMATEO DE CADENAS DE CARACTERES

> **NOTA IMPORTANTE:**
> LAS INSTRUCCIONES DESCRIPTAS A CONTINUACION NO PUEDEN SER PROGRAMADAS NI APLICADAS EN OSIMPLC V2.0.

ESTAS INSTRUCCIONES HAN SIDO DESARROLLADAS PARA PERMITIR LA COMUNICACION DE DATOS POR MEDIO DE LOS BUSES DE DATOS I2C Y SPI EN LOS MICROCONTROLADORES EN LOS CUALES TIENEN SOPORTE, Y PARA OBJETIVOS TIPO NETZER (ETHERNET ON 8 BIT MICROCONTROLLERS) DONDE HA SIDO IMPLEMENTADA LA FUNCION printf().  

HAN SIDO INCLUÍDAS EN ESTE MANUAL A MODO DE ILUSTRACION DE LAS POSIBILIDADES DE PROGRAMACIÓN INCLUIDAS EN LDMICRO, Y PARA MANTENER COMPATIBILIDAD CON EL MANUAL EN ESPAÑOL PROVISTO JUNTO CON EL SOFTWARE.

Razones de la exclusión de estas instrucciones en OSIMPLC:  

1. Los buses I2C y SPI no están regulados bajo la normativa IEC 61131, y no son estándar en implementaciones de autómatas programables del tipo PLC.  
2. En el hardware de OSIMPLC, todos aquellos pines del microcontrolador que ofrecen acceso a periféricos que controlan dichos buses se han implementado como entradas y salidas digitales o como entradas analógicas normalizadas (es decir, como señales estándar en PLCs industriales).  
3. Las instrucciones de control de los buses I2C y SPI en LDmicro sólo están disponibles para microcontroladores de la línea ARM de 32 bits y algunos modelos de la línea AVR ATmega. OSIMPLC se ha desarrollado teniendo como objetivo de primera instancia la utilización de microcontroladores de la línea PIC de Microchip (aunque no se descartan próximas implementaciones con microcontroladores AVR ATmega).  
4. La compilación de las instrucciones de control para los buses I2C y SPI requiere:  
  a) Utilizar un programa compilador externo (AVR-GCC ó ARM-GCC) para recompilar el código generado en lenguaje C por LDmicro junto con las bibliotecas de funciones en C correspondientes, mediante los menús "Compile Atmel AVR-GCC" o "Compile ARM-GCC";  
  b) completar la generación del archivo .hex (código máquina) por medio del menú "FlashMCU" en LDmicro;  
  c) si el correspondiente archivo flashMcu.bat está correctamente configurado, descargar el código máquina en el microcontrolador AVR o ARM utilizando el software de programación ("quemador") y el hardware correspondiente a cada línea (AVR o ARM).  
Estos procedimientos exceden el alcance y los propósitos del proyecto OSIMPLC.  

---

### SPI ENVIAR / RECIBIR
```
      {SPI      SPI1}  
    --{->recv send->}--  
```
Sólo un bus SPI puede ser programado en un mismo esquema Ladder, llamado SPI, o SPI1, SPI2 ó SPI3 cuando hay varios buses implementados en el microcontrolador.  
SPI sólo trabaja en modo maestro, con datos de 8 bits, enviando/recibiendo el MSB primero, y su frecuencia debe ser establecida en los parámetros del microcontrolador mediante el menú Configuraciones > MCU parámetros...  
El pin SS es controlado por el código en los microcontroladores AVR, pero no en los ARM. Puede utilizarse pines externos para activar / desactivar los esclavos en el bus SPI, y/o el pin SS en AVR.  
En un bus SPI, el envío y la recepción son simultáneos, por eso ambas operaciones han sido incluídas en una sola instrucción.  

---

### SPI ESCRIBIR  
```
      {SPI_WR  SPI}  
    --{"Message"->}--  
```
La instrucción SPI_WR envía una cadena de caracteres por el bus SPI, sin tener en consideración la existencia de datos entrantes.  

La cadena puede contener caracteres especiales precedidos por \\\\.  
No abusar de esta instrucción, porque puede ralentizar la ejecución del programa y generar inconvenientes en el control del tiempo de ciclo de PLC.  

---

### I2C LEER  
```
      {I2C_RD    I2C}  
    --{->recv 0x20 0}--  
```
La instrucción I2C READ lee un byte desde la dirección dada del registro en el esclavo especificado.  

---

### I2C WRITE  
```
      {I2C_WR    I2C}  
    --{0x20 0 send->}--
```
La instrucción I2C WRITE escribe un byte a la dirección dada del registro en el esclavo especificado.  

---  

### FORMATEAR CADENA DE CARACTERES  
```
           dest  
    --{"string", var}--  
```
Esta instrucción puede ser usada para generar código compilado para objetivos como Netzer, que cuentan con implementaciones de printf preexistentes.  

Cuando aparece un flanco positivo de la señal en la entrada lógica de esta instrucción (transición de 0 (OFF, False) a 1 (ON, True), comienza a procesar la cadena con printf y escribe el resultado en el registro de destino.  

La instrucción embebe completamente la cadena en la imagen resultante.  

La variable "var" puede ser cualquier registro del programa y es utilizada como marcador de posición para printf. La función printf accede al registro si se encuentra un marcador de posición de estilo printf (p. ej. %d).

dest es un registro donde se escribe el resultado. Éste debería ser la entrada superior de un buffer FIFO o LIFO.  

La salida de la instrucción es siempre 1 (ON, True).  


## NOTA PARA EL USO DE FUNCIONES MATEMATICAS


**Actualización a versiones >= v.4.4.0** 

Recuerde que LDmicro realiza operaciones matemáticas sólo con variables enteras con signo de 8, 16, 24 ó 32 bits.  

Ésto significa que el resultado final de cualquier cálculo que se realice debe ser un entero entre dentro del rango del operando de mayor tamaño utilizado en la instrucción. También significa que los resultados intermedios de los cálculos deben estar dentro de ese rango.

Operaciones aritméticas con variables de distinto tamaño (alcance, span) podrían dar lugar a resultados inesperados.  

Es recomendable que las variables que alojarán a todos los operandos, a los resultados parciales en las sucesivas operaciones intermedias, y al resultado final, posean el mismo tamaño y éste sea igual a o mayor que el de la variable de mayor tamaño.  

**ATENCION:**
Tenga muy en consideración que LDmicro sólo efectúa operaciones con variables de números enteros.  

Por ejemplo, digamos que usted desea calcular temp = (1 / x) * 1200, donde x está entre 1 y 20.  
Entonces y tendrá un resultado entre 1200 y 60, que se ajusta a un entero de 16 bits, por lo que al menos en teoría es posible realizar el cálculo si inconvenientes.  

Hay dos modos de codificar ésto:  

a) Podría realizar el recíproco, y luego multiplicar:
```
  ||  {DIV temp  := }  
  ||--{ 1 / x       }--  
  ||
  ||  {MUL  y    := }  
  ||--{ temp * 1200 }--  
  ||
```
b) Simplemente podría hacer la división directamente, en un solo paso:  

```
  ||  {DIV  y  :=}  
  ||--{ 1200 / x }--  
```

Matemáticamente, estos dos cálculos son equivalentes; pero si usted los intenta, encontrará que el primero modo da un resultado incorrecto de y = 0, salvo en el caso en que x = 1.  

Ésto es debido a que en el modo a) la variable "temp" siempre tendrá un resultado menor que 1 si el dividendo es mayor que 1 (underflow ó desbordamiento inferior), y ese resultado es truncado por la instrucción de la división.  

Por ejemplo, cuando x = 3 => (1 / x) = 0,333, pero este resultado no es un número entero, y  la operación de división aproxima ésto como temp = 0.  
El resultado de la segunda operación, la multiplicación, será y = temp * 1200 = 0, generando un error.  

En el segundo modo, b) no hay ningún resultado intermedio menor a 1 que cause desbordamiento inferior, así que todo funcionará como es esperado.  

Sin embargo, note que los resultados son truncados, lo que significa los decimales serán eliminados en la variable que alojará el resultado.

Si usted está encontrando problemas con sus matemáticas, compruebe los resultados intermedios para verificar que no exista desbordamiento inferior (número menor de la unidad mínima de la variable) o desbordamiento superior, por ejemplo, 32767 + 1 = -32768 si se está trabajando con variables int16 con signo.  

Cuando necesite escalar una variable por algún factor, hágalo usando primero una multiplicación y luego una división, pero siempre verificando que la primera operación no resulte en desbordamiento superior (overflow).  

Por ejemplo, para escalar y = 1,8 * x, calcule y = (9/5) * x (que es el mismo, ya que 1,8 = 9/5), y programe el código de éste como y = (9 * x) / 5, realizando primero la multiplicación:

```
  ||  {MUL  temp  :=}  
  ||--{ x * 9       }--  
  ||  
  ||  {DIV  y  :=}  
  ||--{ temp / 5 }--  
```
Ésto funcionará para todos los valores x < 32767/9, o x < 3640.  
Para valores mayores de x, la variable `temp 'se desbordará. Hay un límite inferior similar en x.  


## ESTILO DE CODIFICACION  

**1.**  

Está permitida la programación de múltiples bobinas u otras operaciones (aritméticas, MOV, etc.) en paralelo en un solo escalón (rung).  

Ésto significa que usted puede hacer programaciones como la siguiente:  
```
  ||        Xa               Ya  
  ||-------] [--------------( )--  
  ||  
  ||        Xb               Yb  
  ||-------] [------+-------( )--  
  ||                |  
  ||                |        Yc  
  ||                +-------( )--  
  ||  
```
que resulta equivalente a ésta:  
```
  ||        Xa               Ya  
  ||-------] [--------------( )--  
  ||
  ||        Xb               Yb  
  ||-------] [--------------( )--  
  ||
  ||        Xb               Yc  
  ||-------] [--------------( )--  
  ||  
```
Ésto significa que en teoría se podría escribir cualquier programa como una sola línea gigante, y no habría necesidad de utilizar múltiples escalones en absoluto.  

En la práctica ésto sería una mala idea, porque como los líneas se vuelven más complejas se hace mucho más difícil de editar sin borrar y redibujar una gran cantidad de lógica.  

Aún así, a menudo es una buena idea agrupar la lógica relacionada en un solo escalón.  

Ésto genera un código casi idéntico a si se hiciera en escalones separados, pero claramente muestra que están relacionados cuando se ven en el diagrama de contactos.  

**2.**  

En general, se considera mala práctica escribir el código de tal manera que su salida dependa del orden de los escalones. Por ejemplo, este código no es muy bueno si tanto Xa como Xb pueden ser verdaderos:

```
  ||        Xa        {v  :=   }  
  ||-------] [--------{ 12  MOV}--  
  ||  
  ||        Xb        {v  :=   }  
  ||-------] [--------{ 23  MOV}--  
  ||  
  ||       [v>]              Yc  
  ||------[ 15]-------------( )-- 
  ||  
```


Sin embargo, puede romper esta regla si al hacerlo logra programar un segmento de código significativamente más compacto o efectivo.  

Por ejemplo, he aquí cómo haría para que el código convierta una cantidad binaria de 4 bits leída en Xb3:0 en una variable de un entero (o de un byte) con signo:

```
  ||                   { v  :=    }  
  ||-------------------{ 0     MOV}--  
  ||  
  ||       Xb0         { ADD  v :=}  
  ||-------] [---------{ v + 1    }--  
  ||  
  ||       Xb1         { ADD  v :=}  
  ||-------] [---------{ v + 2    }--  
  ||
  ||       Xb2         { ADD  v :=}  
  ||-------] [---------{ v + 4    }--  
  ||  
  ||       Xb3         { ADD  v :=}  
  ||-------] [---------{ v + 8    }--  
  ||
```
En el primer escalón, la instrucción MOV pone a cero (resetea) la variable v antes de que las siguientes instrucciones le asignen un nuevo valor en función de las señales leídas en los contactos asignados a entradas físicas. 

Si la instrucción MOV se moviera al escalón inferior de este segmento de programa en lugar de estar ubicada en el escalón superior, entonces el valor de v sería siempre 0 cuando se leyese en cualquier otro sector del programa (antes o después del segmento).  

La salida de este código depende por lo tanto del orden en que se evalúan las instrucciones.  
Teniendo en cuenta lo engorroso que sería codificar ésto de otra manera, es aceptable.

> **NOTA**: en versiones >= 3.X también es posible utilizar instrucciones como SetBit para efectuar la asignación de un 1 en cada bit de la variable destino v.  


## ABREVIATURAS  

* PLC - controlador lógico programable.  
* PWM - modulación de ancho de pulso.  
* ADC - convertidor analógico a digital.  
* UART - Receptor Transmisor Asíncrono Universal  
* PCB - placa de circuito impreso  


## ERRORES  

LDmicro no genera código muy eficiente; es lento de ejecutar, y desperdicia mucho de la capacidad de memoria Flash y RAM. A pesar de esto, un microcontrolador PIC de rango medio o un AVR ATmega pueden ejecutar casi todo lo que un pequeño PLC puede hacer, si se trata de tareas simples.  

La longitud máxima de los nombres de las variables es muy limitada. Ésto es para gráficamente que encajen muy bien en el diagrama Ladder, así que no hay una buena solución a este incoveniente.  

Si su programa es demasiado grande para el tiempo de ciclo de PLC, o las memorias de programa o de datos tienen restricciones en el dispositivo que ha elegido, probablemente no lo obtendrá ningún indicio de error. Muy probablemente, la ejecución del código máquina se "colgará" en algún lugar.  
Se pueden desarrollar técnicas de programación y depuración que le ayuden a resolver ésto.  

La programación descuidada en las rutinas de carga ó almacenamiento de los archivos .ld y .hex hace probable que resulte posible bloquear o ejecutar código arbitrario en el dispositivo, al cargarse un archivo .ld corrupto o generado malintencionadamente.  

Por favor, informe de errores adicionales o solicitudes de características al autor.  

---

Gracias a:  
=========

* Marcelo Solano, por reportar un error de interfaz de usuario bajo Win98  
* Serge V. Polubarjev, por informar que RA3: 0 en elPIC16F628 no funcionaba y también indicar cómo solucionarlo  
* Maxim Ibragimov, por reportar y diagnosticar problemas mayores con los objetivos ATmega16 y ATmega162 hasta entonces no probados  
* Bill Kishonti, por informar que el simulador se colgaba cuando el programa en Ladder efectuaba división por cero  
* Mohamed Tayae, por reportar que las variables persistentes fueron rotas en el PIC16F628  
* David Rothwell, por reportar varios errores en la interfaz de usuario y un problema con la función "Exportar como texto"  


EXENCION DE RESPONSABILIDAD  
==========
  
**NO UTILICE CODIGO GENERADO POR LDMICRO EN APLICACIONES DONDE UNA FALLA EN EL SOFTWARE PODRIA RESULTAR EN PELIGRO A LA VIDA HUMANA O EN DAÑO A LA PROPIEDAD.**  

**EL AUTOR NO ASUME NINGUNA RESPONSABILIDAD POR LOS DAÑOS RESULTANTES DE LA OPERACION DE LDMICRO O POR EL CODIGO GENERADO POR LDMICRO.**  


LICENCIA DE USO, COPIA Y MODIFICACION  
===========

Este programa es software libre: puede usarlo, redistribuirlo y/o modificarlo bajo las condiciones de la Licencia Pública General GNU (GPL) publicada por la Free Software Foundation, ya sea la versión 3 de la Licencia, o a su elección, cualquier versión posterior.  

Este programa se distribuye con la esperanza de que sea útil, pero SIN NINGUNA GARANTIA; sin la garantía implícita de COMERCIABILIDAD O APTITUD PARA UN PROPOSITO PARTICULAR.  

Vea la Licencia Pública General de GNU (GPL) para más detalles.  
Debería haber recibido una copia de la Licencia Pública General de GNU (GPL) junto con este programa. Caso contrario, vea <http://www.gnu.org/licenses/>.

**Jonathan Westhues**  
```
Rijswijk      -- Dec 2004  
Waterloo ON   -- Jun, Jul 2005  
Cambridge MA  -- Sep, Dec 2005; Feb, Mar 2006; Feb 2007  
Seattle WA    -- Feb 2009  

Email: jwesthues at cq dot cx
```

## LDmicro: SOPORTE, INFORMACION, WIKI  

**LDmicro Forum:  <http://cq.cx/ladder-forum.pl>**  


**Actual desarrollador: Ihor Nehrutsa (¡Muchas gracias!!!)**

Última versión: <https://github.com/LDmicro/LDmicro/releases>  

Repositorio: <https://github.com/LDmicro/LDmicro>  

Wiki: <https://github.com/LDmicro/LDmicro/wiki>  

Email: <LDmicro.GitHub@gmail.com>

**Traducción de esta versión y adaptación del manual al español para OSIMPLC:**

Daniel Hernando Mirkin  

Email: danielmirkin at gmail dot com
