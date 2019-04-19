# PROGRAMMING AND TRANSFERRING ENVIRONMENTS

## SOFTWARE, FIRMWARE, HARDWARE

OSIMPLC requires two different environments for its use:

1. Programming environment, in which the application (user program) will be developed and will
be compiled in machine code (.hex file).
2. Transferring environment, which performs the download of the machine code, compiled by
the programming environment, into the microcontroller.

OSIMPLC can be programmed under any software that supports the PIC16F887 (standard)
and/or PIC18F4520 (alternative) microcontrollers, and compiles machine code in .hex files
(I8HEX).

Transferring of the machine code into OSIMPLC can be done, either by means of serial
communication using a bootloader, the UART connector and an USB-TTL converter, or by
direct transferring using a PIC programmer, using any hardware and its corresponding software
that support the aforementioned microcontrollers.

These software can be free (as libre) licensed (GPL, BSD, MIT, or compatible), free (as gratis)
proprietary license (MPLAB or others), or paid proprietary license.

However, being OSIMPLC an Open Hardware + Free Software project, we recommend only its
use with software with free (as libre) license, and will not provide support for any application or
development based on proprietary software.

## RECOMMENDED PROGRAMMING ENVIRONMENTS

LADDER (IEC 61131-3): [LDmicro](07-ldmicro.md), GPLv3 license 

[Other languages](08-otherlangs.md) not included in IEC61131-3

BASIC: Great Cow Basic, GPLv2 license, LGPLv2.
C: SDCC, GPLv2+l icense, + IDE (Code::Blocks, Eclipse, others).

ASSEMBLER: gpasm (gputils suite), GPLv2 license, + text editor (on GNU/Linux, Geany can be
used as IDE).

## RECOMMENDED TRANSFERRING ENVIRONMENTS

### 1. SERIAL TRANSFERRING: SOFTWARE, FIRMWARE, HARDWARE

#### GNU/Linux Software

OBootLin: GPLv2 license, based on Python3 (link!!)

Prerequisites: you must install `python3-serial` and `python3-wxphython` dependencies, provided
by your distribution.

> **Note**:
> This software, available as OBootLin in the [Downloads](http://osimplc.com/downloads) area, is a modification of the tinybldlin
> software (fork lcgamboa) for use with OSIMPLC; its directories contain only the identify modules
> and bootloaders for PIC16F887 and PIC18F4520 with 20 MHz crystal oscillator and serial
> communication on 19200-8N1 standard.

#### Software for Windows

[Tiny Multi Bootlader +](http://tinypicbootload.sourceforge.net/): Creative Commons Attribute - Non-Commercial License (CC BY-NC)

This software allows trasferring of machine code into different lines of microcontrollers. Read
the installation and usage instructions on its website.

#### Firmware

Tiny PIC Bootloader firmwares for PIC16F887 and PIC18F4520, pre-installed in the OSIMPLC
microcontrollers, have been modified for compatibility with 20 MHz oscillator crystal and serial
communication in 19200-8N1 standard.

#### Hardware

The OSIMPLC project provides a programming cable that includes the USB-TTL converter
based on CH340 or CH341 chipset, and a female socket terminal already arranged for
connection to the OSIMPLC UART pin-out.

However user can, at their own risk, to use a generic USB-TTL converter and build their own
programming cable with the appropriate connector.
Modern operating systems like GNU/Linux (kernel> = 4.0) and Windows (8/10) already include
drivers for USB-TTL converters based on chipsets CH340, CH341, CP2102, FTDI232 and
Prolific2303.

In case the S.O. don't have such drivers, you should consult the USB-TTL converter provider
about the included chipset and the recommended driver for that component.

### 2. DIRECT TRANSFERRING (PICs PROGRAMMER HARDWARE)

OSIMPLC recommends the use of [USBPICPROG](http://usbpicprog.org/), USB port programmer, Open Hardware + Free Software licensed.

The machine code compiled by the Programming Environment (.hex file) can be transferred into
OSIMPLC through its ICSP connector, using USBPICPROG and a cable with the appropriate
pin-out.

USBPICPROG software is distributed with GPLv2 license, you can download the source code
for GNU/Linux, and installers for the Ubuntu, Windows and MacOSX distribution.

For the Arch Linux distribution, [an USBPICPROG PKGBUILD is available in AUR](https://aur.archlinux.org/packages/usbpicprog/), written by the OSIMPLC developer.

USBPICPROG has a hardware version based on double layer through hole printed circuit board
(Two Layer Through Hole PCB), and standard electronic components available in the local
market, so it can be built and assembled by the user.

> **Note**:
> Direct transferrring of machine code to OSIMPLC, using a PIC programmer, will overwrite
> (erase) the pre-installed bootloader.
> If is required the reinstallation of the bootloader to enable serial programming again, the
>programmer must be used to transfer previously the firmware corresponding to the installed
> microcontroller model through ICSP.
