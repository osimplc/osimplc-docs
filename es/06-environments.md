# ENTORNOS DE PROGRAMACIÓN Y TRANSFERENCIA

OSIMPLC requiere dos entornos diferentes para su utilización:

1. Entorno de programación, en el que se desarrollará la aplicación (programa de usuario) y se
procederá a su compilación en código máquina (archivo .hex).
2. Entorno de transferencia, que efectúa la descarga en el microcontrolador del código máquina
compilado por el entorno de programación.

OSIMPLC puede ser programado bajo cualquier software que soporte los microcontroladores
PIC16F887 (estándar) y/o PIC18F4520 (alternativo), y genere códico máquina en archivos .hex
(I8HEX).

Dichos softwares pueden ser de licencia libre (Free Software GPL, BSD, MIT, o compatibles),
de licencia privativa gratuita (MPLAB u otros), o de licencia privativa paga.

Sin embargo, siendo OSIMPLC un proyecto Open Hardware + Free Software, recomendamos
únicamente su utilización con software de licencia libre, y no brindaremos soporte para ninguna
aplicación o desarrollo basado en software privativo.

## ENTORNOS DE PROGRAMACIÓN RECOMENDADOS

LADDER (IEC 61131-3): [LDmicro](07-ldmicro.md), licencia GPLv3

[Otros lenguajes](08-otherlangs.md) no incluídos en normativa IEC61131-3:

* BASIC: Great Cow Basic, licencia GPLv2, LGPLv2.

* C: SDCC, licencia GPLv2 o GPLv3, + IDE (Code::Blocks, Eclipse, otros).

* ASSEMBLER: gpasm (suite gputils), licencia GPLv2, + editor de texto (sobre GNU/Linux se puede utilizar Geany como IDE).

## ENTORNOS DE TRANSFERENCIA RECOMENDADOS

La transferencia del código máquina al OSIMPLC puede ser realizada, ya sea por medio de
comunicación serial utilizando un bootloader, el conector UART y un conversor USB-TTL, o por transferencia directa utilizando un programador de PICs, utilizando cualquier hardware y su
correspondiente software que soporte los antedichos microcontroladores.

### 1. TRANSFERENCIA SERIAL: SOFTWARE, FIRMWARE, HARDWARE

#### Software para GNU/Linux

OBootLin: licencia GPLv2, basado en Python3 (link!!).

Pre-requisitos: deberá instalar como dependencias `python3-serial` y `python3-wxphython`,
provistos por su distribución.

> **Nota**:
> Este software, disponible como OBootlin en el área de [Descargas](http://osimplc.com/downloads), es una modificación del
> software tinybldlin (fork lcgamboa) para su utilización con OSIMPLC; sus directorios contienen
> únicamente los módulos identificadores y los bootloaders para PIC16F887 y PIC18F4520 con
> cristal oscilador de 20 MHz y norma de comunicación serial 19200-8N1.

#### Software para Windows

[Tiny Multi Bootlader+](http://tinypicbootload.sourceforge.net/): Licencia Creative Commons Attribute - No Comercial (CC BY-NC)

Este software permite la descarga del código máquina en diversas líneas de
microcontroladores. Lea las instrucciones de instalación y uso en su sitio web.

#### Firmware

Los firmwares de Tiny PIC Bootloader para PIC16F887y PIC18F4520, pre-instalados en los
microcontroladores de OSIMPLC, han sido modificados para su compatibilidad con cristal
oscilador de 20 MHz y norma de comunicación serial 19200-8N1.

#### Hardware 

El proyecto OSIMPLC provee un cable de programación que incluye el conversor USB-TTL
basado en chipset CH340 ó CH341, y un terminal zócalo hembra ya dispuesto para conexión
con el pin-out UART de OSIMPLC.

Sin embargo, el usuario puede, bajo su propia responsabilidad, utilizar un conversor USB-TTL
genérico y construir su propio cable de programación con los terminales adecuados.
Los modernos sistemas operativos GNU/Linux (kernel >=4.0) y Windows (8 / 10) ya incluyen los
drivers para los conversores USB-TTL basados en los chipsets CH340, CH341, CP2102,
FTDI232 y Prolific2303.

En caso de que el S.O. no disponga de dichos drivers, se debe consultar al proveedor del
conversor USB-TTL acerca del chipset incluído y el driver recomendado para ese componente.

### 2. TRANSFERENCIA DIRECTA (HARDWARE PROGAMADOR DE PICs)

OSIMPLC recomienda el uso de [USBPICPROG](http://usbpicprog.org/), programador por puerto USB, Open Hardware + Free Software

El código máquina compilado por el Entorno de Programación (archivo .hex) puede ser
transferido a OSIMPLC por medio de su conector ICSP, utilizando USBPICPROG y un cable
con el pin-out adecuado.

El software de USBPICPROG se distribuye con licencia GPLv2, puede descargarse el código
fuente para GNU/Linux, e instaladores para la distribución Ubuntu, Windows y MacOSX.

Para la distribución Arch Linux, [está disponible en AUR el PKGBUILD que permite la compilación de USBPICPROG](https://aur.archlinux.org/packages/usbpicprog/), escrito por propio desarrollador de OSIMPLC.

USBPICPROG dispone de una versión de hardware basada en circuito impreso doble capa y
agujero pasante (Two Layer Through Hole PCB), y componentes electrónicos estándar
disponibles en el mercado local, por lo que puede ser construído y montado por el propio
usuario.

> **Nota**:
> La transferencia directa del código máquina a OSIMPLC, por medio de un programador de
> PICs, sobreescribirá (borrará) el bootloader pre-instalado.
> En caso de requerirse la reinstalación del bootloader para habilitar nuevamente la
> programación serial, deberá utilizarse el programador para transferir previamente el firmware
> correspondiente al modelo de microcontrolador instalado por medio de ICSP.
