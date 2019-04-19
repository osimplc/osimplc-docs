HMI MODULE
==========

![HMI](images/hmi.png)

The HMI module (Human Machine Interface) is designed to facilitate the interaction between
user and the automatism; it allows to display text messages for alarm or information, status of
digital inputs and outputs, numeric variables (scaled values from analog inputs, timers,
counters, internal variables, etc.), make changes in the working parameters, and execute
control commands .

It consists in a LCD display of 2 rows by 16 characters with backlighting and four programmable
pushbuttons; it does not have its own memory;and communicates with MCU module via TTL
serial bus, being able to be mounted at short distance from OSIMPLC, i.e. in the front of
cabinet.

Interaction between the MCU module and the HMI module is controlled by means UART
operations in 96008N norm from the user program loaded in the microcontroller, supporting a
basic subset of Matrix Orbital serial commands.

If PIC16F887 microcontroller is installed in the MCU module and the programming under Ladder
is developed using LDmicro software, the interaction capacity will be considerably limited, and
must be carefully optimized by the programmer.

Interaction possibilities will be significantly increased if the PIC18F4520 microcontroller is
installed in the MCU module and programming is developed under BASIC, C or Assembler,
taking advantage of libraries and specialized functions available in these languages.

> **NOTE**:
> The HMI module is not an industrial-type operator panel (OP); it does not currently have an
> enclosure or cabinet with IP protection grade (in development).
