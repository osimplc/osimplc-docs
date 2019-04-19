# OUTPUT MODULES

OSIMPLC outputs to industrial field signals are available through four-way dockable modules.

These outputs are suitable to command actuators or intermediate maneuvering equipment that
operate only under safety voltage (24V), is not allowed to connect elements that work in mains
voltage.

OSIMPLC [MCU](02-mcu.md) has three ports, Y0-Y3, Y4-Y7 and Y8-YB/PWM, for output modules direct connection; some of these (only with digital outputs) can be indistinctly connected to any of the
three ports, while other modules (with analog or PWM output) can only be connected to the Y8-
YB/PWM port.

All output modules require external power, have a fuse for its control circuit, and fuse protection
for each of its digital outputs (except in frequency inverter control module), and in its analog output, in those in which is present.

Digital outputs have limited protections against power surges and reverse voltage peaks caused
by the disconnection of inductive loads.

However, the user must install external protections corresponding to this type of loads (RC
network, varistor, for inductive loads in AC; freewheel diode for inductive loads in DC).

## O4R: RELAY OUTPUTS MODULE

Four (4) relay outputs, NO contact, Unom: 24 Vac / cc; Imax: 2A resistive load, 1A
inductive load.

Module power supply: 24Vdc (standard), 12Vdc (O4R-12, on request). User can share the
power supply with the MCU module, or use independent source.

![O4R](images/o4r.png)

## O4T: POWER TRANSISTOR OUTPUT MODULE

Four (4) transistor outputs type NPN open collector (MOSFET N channel), Unom: 24 Vcc;
Imax: 2A resistive load, 1A inductive load.

Module power supply: 24 /12Vdc, configurable by jumper. User can share the power
supply with the MCU module, or use independent source.

![O4t](images/o4t.png)

## OMX: MIXED OUTPUTS MODULE, RELAY + TRANSISTOR + ANALOGUE OUTPUT

Two (2) relay outputs, NO contact, Unom: 24 Vac/cc; Imax: 2A resistive load, 1A inductive
load.

One (1) NPN open collector transistor output, Unom: 24 Vdc; Imax: 200 mA resistive load,
100 mA inductive load.

One (1) analog output, 0-10V / 0(4) -20mA configurable, max. 10 bits resolution.

Module power supply: 24Vdc. User can share the power supply with the MCU module, or
use independent source.

![OMX](images/omx.png)

## OVF: MODULE FOR CONTROL OF FREQUENCY VARIABLE

Three (3) outputs for direct connection of the digital inputs of frequency inverter to
optocouplers in the MCU module. They can be used indistinctly with PNP or NPN signals.

Not protected by fuse.

Una (1) salida analógica, normas 0-10V / 0(4)-20mA, resolución máx. 10 bits.

One (1) analog output, 0-10V / 0(4) -20mA configurable, max. 10 bits resolution.

Module power supply: 24Vdc. User can share the power supply with the MCU module, or
use independent source.

![OVF](images/ovf.png)

## UNDER DEVELOPMENT

### ODCM: MODULE FOR DC MOTOR CONTROL

H Bridge outputs, current up to 10A @ 24Vdc, plus one (1) transistor output type NPN
open collector (MOSFET N channel), Unom: 24 Vdc; Imax: 2A resistive load, 1A inductive
load; allows rotation reversion, speed control by PWM, braking/freewheeling of the motor,
and electromechanical brake actuation.
