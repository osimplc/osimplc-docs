# LDmicro

LDmicro es el software recomendado para el uso de OSIMPLC en entornos educativos, ya que
facilita el aprendizaje del lenguaje de programación para PLCs más habitual en entornos
industriales: LADDER (IEC 61131-3).

También es el software más conveniente para el rápido desarrollo de aplicaciones simples
destinadas a entornos productivos, gracias a su editor gráfico simplificado, las instrucciones
estándar integradas, fácil configuración de puntos de Entradas/Salidas y módulos de
expansión, y disponibilidad de rutinas de comunicación básicas.

El microcontrolador PIC16F887 incluído en OSIMPLC (estándar) está plenamente soportado
por LDmicro, no así el microcontrolador PIC18F4520 (alternativo), cuyo juego de instrucciones
es diferente y debe ser programado utilizando otros lenguajes: BASIC, C, Assembler.

LDmicro corre nativamente sobre Windows; y también sobre GNU/Linux bajo Wine, sin
necesidad de utilizar DLL externas o no libres.

LDmicro no requiere instalación, simplemente descargar el archivo, descomprimirlo en un
directorio y ejecutar el .exe correspondiente. Dispone de distintos ejecutables con la traducción
de los menús y diálogos a diversos idiomas, incluído por supuesto el español.

## Instrucciones disponibles en LDmicro

Contactos y bobinas, normales e invertidos.

Evaluación de flanco ascendente (positivo) y descendente (negativo).

Operaciones sobre bobinas: Set, Reset, Trigger (telerruptor).

Lectura de entradas analógicas (ADC).

Control de salida PWM.

Temporizadores: a la Conexión, a la Desconexión, de Impulso, Retentivo, Cíclico.

Contadores: Ascendente, Descendente, Bidireccional, Cíclico, Slow Quad Encoder.

Operaciones aritméticas en variables con signo: suma, resta, multiplicación, división,
módulo, negación, mover variable.

Operaciones de comparación sobre variables con signo: >, <, =, >=, <=, !=.

Operaciones bit-a-bit sobre variables sin signo: AND, OR, NOT, XOR, desplazamiento
lineal y cíclico, inversión, swap.

Operaciones sobre bits individuales en variables: Test Bit Set, Test Bit Clear, Set Bit,
Clear Bit.

Operaciones especiales con variables: direccionamiento indirecto, desplazamiento de
variable indexada, linearización, tabla de búsqueda, generación de número aleatorio,
persistencia de variable (escritura en EEPROM).

Operaciones de conversión: BIN <-> BCD.

Operaciones de control del programa: Master Control Relay, GOTO, GOSUB y
complementarias, SLEEP, DELAYuS, CLRWDT.

Operaciones de comunicación sobre UART: enviar cadena de caracteres y/o variable,
recibir carácter, enviar carácter, control de buffers.

LDmicro permite compilar el programa de usuario (aplicación) desarrollado bajo Ladder
generando un archivo .hex que contiene el código máquina para algunos dispositivos de dos
líneas de microcontroladores de 8 bits: Microchip PIC y AVR Atmega; y en una versión
derivada, también para un microcontrolador STM32F40X ARM de 32 bits.

Además, LDmicro permite generar código en Assembler, C, PASCAL, Interpretable Byte Code,
Sketch para Arduino.

## Simulación

LDmicro habilita la simulación de la ejecución del programa de aplicación en la PC, sin
necesidad de compilar y descargar el código máquina en el microcontrolador, facilitando así la
corrección del programa desarrollado en lenguaje Ladder.
Sin embargo, esa herramienta debe utilizarse con precaución, ya que la simulación no puede
reproducir en su totalidad las subrutinas y tiempos de ejecución de las microinstrucciones
nativas del controlador.


## VERSIONES DE LDMICRO

### ORIGINAL

LDmicro fue desarrollado inicialmente por Jonathan Wuesthues en el año 2005, quien mantuvo
sus actualizaciones hasta la versión v2.3 (02/01/2016).

LDmicro es Free Software y fue desarrollado bajo licencia GPL versión 3, por lo que el libre
uso, distribución, y modificación del código fuente y de los ejecutables está garantizada.

El sitio web http://cq.cx/ladder.pl contiene valiosa información sobre instalación y uso del
programa, tutoriales, notas técnicas, y links para descarga de la versión v2.3 y anteriores.

En su foro http://cq.cx/ladder-forum.pl , una activa comunidad de usuarios intercambia
información, difunde ejemplos de programación, responde consultas, y es una fuente
inestimable para la resolución de problemas en el uso del programa y en la implementación de
aplicaciones.

### ACTUAL

LDmicro es actualmente mantenido por Ihor Nehrutsa, quien ha agregado soporte para nuevos
microcontroladores PIC y AVR Atmega, e implementaciones como Arduino y Controllino Maxi,
además de muchas nuevas funciones en Ladder y características avanzadas.

El desarrollo de LDmicro se realiza actualmente en el sitio https://github.com/LDmicro/LDmicro ;
la descarga del código fuente y los ejecutables puede hacerse desde https://github.com/LDmicro/LDmicro/releases

Ihor Nehrutsa mantiene también una importante wiki en https://github.com/LDmicro/LDmicro/wiki , allí encontrará valiosa información sobre las nuevas
funciones, ejemplos, métodos, tutoriales, etc.

> NOTA: La última versión disponible es la v4.4.3.0 (07/08/2018); dado que la misma contiene un
> bug que cierra el programa durante la simulación, es preferible utilizar la versión anterior
> v4.4.2.0 (26/07/2018).

### DERIVADAS

Actualmente hay una versión derivada (fork) de LDmicro: LDmicro32, desarrollada por José
Gilles en https://github.com/joegil95 .

En este nuevo desarrollo, José Gilles ha añadido nuevas funciones para comunicaciones
utilizando los buses I2C y SPI en microcontroladores PIC, AVR y ARM (sólo disponibles
compilando el Ladder en código intermedio en C). Además, ha resuelto el bug de
simulación de la versión v4.4.3.0 y ha reordenado los menús del programa de modo
más funcional.

LDmicro32 añade soporte para el microcontrolador STM32F407 ARM de 32 bits, para el cual
el programa genera código intermedio en C, para luego compilar en código máquina
utilizando el compilador libre ARM-GCC (externo).

La última versión de este fork puede ser descargada desde
https://github.com/joegil95/LdMicro32/

## INSTALACIÓN

### Microsoft Windows

1. Descargue el archivo .zip de la versión de LDmicro de su preferencia y
descomprímalo en una carpeta de su directorio personal.
2. Abra dicha carpeta y ejecute mediante doble click el .exe compilado con el lenguaje
de su preferencia. Opcionalmente, puede crear un acceso directo al ejecutable en su
escritorio y/o en otra/s carpeta/s.

### GNU/Linux

1. Pre-requisitos:
Si utiliza un S.O. de 64 bits, habilite las bilbliotecas multiarquitectura de Intel 32 bits
en su sistema (siguiendo las instrucciones de su distribución). Efectúe una actualización
completa de su sistema.
Si utiliza un S.O. de 32 bits, no es necesario ejecutar este paso.
2. Mediante el gestor de paquetes de su S.O., instale el programa [wine](http://winehq.org) provisto por su
distribución.
3. Utilizando una terminal virtual como usuario, cree y configure en 32 bits el prefijo
wine por defecto:
`$ WINEARCH=win32 winecfg`
Será creado el directorio `/home/$USER/.wine` , conteniendo dos subdirectorios:
`/dosdevices` (links a dispositivos) y `/drive_c` (ejecutables de wine, programas,
configuraciones, etc. etc.).
5. Descargue el archivo .zip de la versión de LDmicro de su preferencia y
descomprímalo en una carpeta de su directorio personal.
6. Utilizando su gestor de archivos, lance el ejecutable de LDmicro en el lenguaje de su
preferencia mediante doble click. El sistema ejecutará LDmicro bajo el entorno wine.
Opcionalmente, utilizando un editor de textos, puede crear un lanzador
LDmicro.desktop en su escritorio y/o en otro/s directorio/s:

```
[Desktop Entry]
Name=LDmicro
Exec=env WINEPREFIX="/home/$USER/.wine" wine
/ruta/al/ejecutable/LDmicro/ldmicroxxx.exe
Type=Application
StartupNotify=true
Path=/ruta/al/directorio/LDmicro/
Icon=/ruta/al/archivo/ldmicro.png (creado o seleccionado por el usuario)
```

## DESCARGA

La descarga de la versión recomendada de LDmicro está directamente disponible [desde el sitio web de OSIMPLC](http://osimplc.com/downloads), es aquella que a nuestro entender brinda mayor estabilidad y mejores
prestaciones (instrucciones integradas, organización de menús, etc.).

Sin embargo, el usuario puede optar por utilizar cualquiera de las otras versiones
descargándolas directamente desde su sitio web, a condición de que en caso de consultas
técnicas por mal funcionamiento, falta de funcionalidad o cualquier otro tipo de inconveniente,
consigne claramente la versión del software que está utilizando.

El manual de LDmicro está incluído en el ejecutable, también es posible descargarlo como un
archivo .txt independiente.


RENUNCIA DE GARANTIA Y LIMITACION DE RESPONSABILIDAD
==========

LDmicro es software libre bajo licencia GPLv3, por lo que en lo referido a garantía y
responsabilidad legal se aplican todas las consideraciones de dicha licencia.

Además, de acuerdo a lo declarado por su desarrollador Jonathan Westhues: "**no** use LDmicro
para nada que sea crítico para la seguridad, o algo que rompa cualquier cosa cara si fallara".

Por supuesto, **NO** debe utilizar LDmicro en conjunto con OSIMPLC en ningún caso en que
pudiere verse afectada la seguridad, la salud o la vida de las personas.

