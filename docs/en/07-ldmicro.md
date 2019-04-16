# LDmicro

LDmicro is the recommended software for OSIMPLC usage in educational environments, since
it facilitates the learning of the most common programming language for PLCs in industrial
environments: LADDER (IEC 61131-3).

Also, it is the most convenient software for quick development of simple applications for
productive environments, thanks to its simplified graphical editor, integrated standard
instructions, easy configuration of Input/Output points and expansion modules, and basic
communication routines availability.

PIC16F887 microcontroller included in OSIMPLC (standard) is fully supported by LDmicro, not
the PIC18F4520 microcontroller (alternative), whose set of instructions is different and can be
programmed using other languages (BASIC, C, Assembler) and different softwares.

LDmicro runs natively on Windows; and also on GNU / Linux under Wine, without needing to
install external or non-free DLLs.

LDmicro does not require installation, simply download the file, unzip it into a directory and
execute the corresponding .exe. It has different executables with tmenus and dialogues
translations for different languages including Spanish, of course.

## Instructions available in LDmicro

Contacts and coils, normal and inverted.

Evaluation of ascending (positive)and descending edges.

Operations on coils: Set, Reset, Trigger.

Analog inputs reading (ADC).

PWM output control.

Timers:On delay, Off delay, One shot, Retentive On delay, Cyclic.

Counters: Ascending, Descending, Bidirectional, Retentive, Cyclic, Slow Quad Encoder.

Arithmetic operations on signed variables: addition, subtraction, multiplication, division,
module, negation, move variable.

Comparison operations on signed variables:>, <, =,> =, <=,! =.

Bit-by-bit operations on unsigned variables: AND, OR, NOT, XOR, linear and cyclic shift
register, inversion, swap.

Operations on individual bits in variables: Test Bit Set, Test Bit Clear, Set Bit, Clear Bit.
Special operations with variables: indirect addressing, indexed variable shift,
linearization, lookup table, random number generation, variable persistence (writing in
EEPROM).

Conversion operations: BIN <-> BCD.

Program control operations: Master Control Relay, GOTO, GOSUB and complementary,
SLEEP, DELAYuS, CLRWDT.

Communication operations on UART: send string and/or variable, receive character,
send character, buffers control.

LDmicro allows to compile the user program (application) developed under Ladder, generating a
.hex file that contains machine code for some devices of two lines of 8-bit microcontrollers:
Microchip PIC and AVR Atmega; and in a derivative version, also for a STM32F40X 32-bit ARM
microcontroller.

In addition, LDmicro allows to generate code in Assembler, C, PASCAL, Interpretable Byte
Code, Sketch for Arduino.

## Simulation

LDmicro enables the simulation of the execution of the application program in the PC, without
the need to compile and download the machine code in the microcontroller, thus facilitating
debug of the program developed in Ladder language.

However, this tool should be used with caution, since the simulation can not reproduce enterely
the subroutines and execution times of native microinstructions in the microcontroller.

## LDMICRO VERSIONS

### ORIGINAL

LDmicro was initially developed by Jonathan Wuesthues in 2005, who maintained it up to
version v2.3 (02/01/2016).

LDmicro is Free Software developed under GPL version 3 license, so the free use, distribution,
and modification of source code and executables is guaranteed.

The website http://cq.cx/ladder.pl contains valuable information on the installation and use of the
program, tutorials, technical notes, and links for downloading of version v2.3 and earlier.

In its forum http://cq.cx/ladder-forum.pl, an active community of users share information,
disseminates programming examples, answers queries, and is an invaluable source for solving
problems in software usage and implementation of applications.

### CURRENT

LDmicro is currently maintained by Ihor Nehrutsa, who has added support for new PIC and AVR
Atmega microcontrollers, and implementations such as Arduino and Controllino Maxi, as well as
many new Ladder instructions and advanced features.

The development of LDmicro is currently carried out on the site
https://github.com/LDmicro/LDmicro; download of source code and executables can be done
from https://github.com/LDmicro/LDmicro/releases.

Ihor Nehrutsa also maintains an important wiki at https://github.com/LDmicro/LDmicro/wiki,
there you will find valuable information about new functions, examples, methods, tutorials, etc.

> NOTE: The latest version available is v4.4.3.0 (07/08/2018); due it contains a bug that closes
> the program during the simulation, it is preferable to use the previous version v4.4.2.0 (07/26/2018).

### DERIVATIVES

Currently there is a derivative version (fork) of LDmicro: LDmicro32, developed by José
Gilles at https://github.com/joegil95

In this new development, José Gilles has added new functions for communications
using the I2C and SPI buses in PIC, AVR and ARM microcontrollers (only available
compiling the Ladder in C intermediate code). In addition, it has solved the version
v4.4.3.0 simulation bug and has reordered the program menus in a more functional
manner.

LDmicro32 adds support for the STM32F407 ARM 32-bit microcontroller, for which the
program generates intermediate code in C, and then compiles in machine code using
the free ARM-GCC compiler (external).

The latest version of this fork can be downloaded from
https://github.com/joegil95/LdMicro32/.

## INSTALL

### Microsoft Windows

1. Download the .zip file of LDmicro version of your choice, and unzip it into a folder in your
personal directory.
2. Open the folder and run the compiled .exe containing the language of your choice. Optionally,
you can create shortcut(s) to the executable in your Desktop and/or other folder(s).

### GNU / Linux

1. Pre-requisites:
If you use a 64-bits OS. enable Intel 32-bits multi-architecture libraries on your system (following
instructions of your distribution). Perform a full update of your system.
If you use a 32 bits OS, is not necessary to execute this step.
2. Using the package manager of your OS, install the [wine](http://winehq.org) program provided by your
distribution.
3. Using a virtual terminal as normal user, create and configure a 32 bits default wine prefix:
`$ WINEARCH = win32 winecfg`
The `/home/$USER/.wine directory` will be created, containing two subdirectories: `/dosdevices`
(symlinks to devices) and `/drive_c` (wine executables, programs, configurations, register, etc.
etc.).
5. Download the .zip file of LDmicro version of your choice, and unzip it to a folder in your
personal directory.
6. Using your file manager, by double clicking the .exe, launch the LDmicro executable in the
language of your choice. The system will run LDmicro under the wine environment.
Optionally, using a text editor you can create a LDmicro.desktop launcher on your desktop
and/or in other directories:

```
[Desktop Entry]
Name = LDmicro
Exec = env WINEPREFIX = "/home/$ USER/.wine" wine
/path/to/executable/LDmicro/ldmicroxxx.exe
Type = Application
StartupNotify = true
Path = /path/to/directory/LDmicro/
Icon = /path/to/file/ldmicro.png (created or selected by the user)
```

## DOWNLOAD

Download of the recommended version of LDmicro is directly available from the [OSIMPLC website](http://osimplc.com/downloads), which is the one that, in our opinion, provides greater stability and better features
(integrated instructions, menu organization, etc.).

However, the user can choose to use any version by downloading them directly from their
website, provided that in case of technical queries due to malfunction, lack of functionality or
any other type of inconvenience, clearly state the software version that is used.

The LDmicro manual is included in the executable, is also possible to download it as a separate
.txt file.

DISCLAIMER OF WARRANTY AND LIMITATION OF LIABILITY:
==========

LDmicro is free software under GPLv3 license, so as regards on warranty and legal liability, all
the considerations of said license will be applied.

In addition, according to the statement by its developer Jonathan Westhues: "**do not** use
LDmicro for anything that is critical for security, or something that breaks anything expensive if it
fails".

Of course, you must **NOT** use LDmicro in conjunction with OSIMPLC in any case where safety,
health or life of the people may be affected.
