# ENTORNOS DE PROGRAMACIÓN: OTROS LENGUAJES

## Lenguaje BASIC

[Great Cow BASIC (GCB)](http://gcbasic.sourceforge.net/), licencia GPLv2, LGPLv2.

> "Great Cow BASIC tiene tres objetivos principales: eliminar la necesidad de repetitivos
>comandos en Assembler, producir código eficiente, y facilitar tomar el código escrito para un
> tipo de microcontrolador y ejecutarlo en otro tipo de microcontrolador".

> "Great Cow BASIC es adecuado para principiantes, para quienes no les gusta o no desean
> aprender el lenguaje Assembler, y para programadores experimentados con
> microcontroladores".

Instalado sobre Windows incluye GCB@Syn IDE, un editor de texto integrado basado en
SynWrite (licencia MPL v1.1); sobre GNU/Linux se puede utilizar GCB Compiler con Geany
como IDE (requier Freebasic como dependencia), o ejecutarlo bajo wine.

GCB dispone de extensas bibliotecas integradas para dispositivos externos, y permite al
usuario incluir sus propias bibliotecas. Además, permite insertar código desarrollado en
Assembler dentro del código en BASIC.

GCB cuenta con una extensa ayuda online, facilitando el aprendizaje y el uso del lenguaje
BASIC, y del programa y sus bibliotecas.

## Lenguaje C

[Small Device C Compiler (SDCC)](http://sdcc.sourceforge.net/), licencia GPLv2 o GPLv3, + IDE.

> "SDCC es un compilador optimizador del Estándar C (ANSI C89, ISO C99, ISO C11) que se
> enfoca en los microprocesadores basados en Intel MCS51, Maxim DS80C390, Freescale
> basados en HC08, MCU basadas en Zilog Z80 y STMicroelectronics STM8.
> 
> El soporte de los microcontroladores Microchip PIC16 y PIC18 en un trabajo en progreso".

La suite SDCC es una colección de varios componentes derivados de diferentes fuentes con
diferentes licencias Free Open Source Software.

SDCC no provee un IDE nativo, pero es plenamente soportado por Code::Blocks en su versión
V17.2, y por Eclipse IDE por medio de un plugin.

## Lenguaje ASSEMBLER

gpasm (suite gputils), licencia GPLv2, + editor de texto.

La suite gputils incluye gpasm, un compilador para Assembler desde línea de comandos (no
dispone de GUI). Puede utilizarse con Geany como IDE: tiene una función de resaltado de
sintaxis para Assembler, y acepta gpasm como compilador desde su menú Build.
