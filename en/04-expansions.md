# EXPANSION MODULES

The onboard expansion modules allow adding more inputs and outputs to the system, reaching
the configurations of 18E / 12S, 16E / 14S or 14E / 16S (30 points).

Most modules take power from the MCU module, with exceptions in certain specialized
modules.

## INPUT MODULES

### E4AI
Four (4) standardized analog inputs, individually configurable in 0-10V, 0-5V or 0(4)-20mA
standards by means of jumpers.

![E4AI](images/e4ai.png)

### E4DI

Four (4) digital inputs, individually configurable as PNP (sink) or NPN (source) by means of
jumpers, with LEDs signaling.

![E4DI](images/e4di.png)

### E2AI2DI

Two (2) standardized analog inputs, individually configurable in 0-10V, 0-5V or 0(4)-20mA
standards by means of jumpers; plus two (2) digital inputs, individually configurable as
PNP (sink) or NPN (source) by means of jumpers, with LEDs signaling.

![E2AI2DI](images/e2ai2di.png)

### E4TI

Specialized expansion for temperature sensing.
Two (2) inputs for connection of PT100 probes (provides 1 mA excitation for each of them);
plus two (2) inputs for connection of NTC-10K probes.

![E4TI](images/e4ti.png)

### E4CI

Specialized expansion for detection of conductive liquids.
Four (4) digital inputs, very low AC current, signaled by LEDs; provides excitation voltage
at 12 Vca @ 900 Hz (minimizing hydrolysis) through terminals for reference electrodes
connection. Signals sent to the microcontroller are inverted respect to liquid detection.

![E4CI](images/e4ci.png)


> **Attention**: this module requires external power in 24Vdc. User can share the power supply
> with the MCU module, or use independent source.

## OUTPUTS MODULE

### E4DO

Four (4) transistor outputs NPN open collector, Umax: 24 Vdc; Imax: 200 mA resistive
load, 100 mA inductive load, with LED signaling (see Note).

![E4TI](images/e4do.png)


## INPUTS/OUTPUS MIXED MODULES

### E2AI2DO

Two (2) standardized analog inputs, individually configurable in 0-10V, 0-5V or 0(4)-20mA
standards by means of jumpers; plus two (2) transistor outputs NPN open collector, Umax:
24 Vdc; Imax: 200 mA resistive load, 100 mA inductive load, with LED signaling (see
Note).

![E2AI2DO](images/e2ai2do.png)


### E2DI2DO

Two (2) digital inputs, individually configurable as PNP (sink) or NPN (source) by means of
jumpers, with LEDs signaling.; plus two (2) transistor outputs NPN open collector, Umax:
24 Vdc; Imax: 200 mA resistive load, 100 mA inductive load, with LED signaling (see
Note).

![E2DI2DO](images/e2di2do.png)

> **Note**:
> Expansion modules do not have protection fuses in their individual outputs, they must be
externally installed by the user.
> Digital outputs have limited protections against power surges and reverse voltage peaks caused
by the disconnection of inductive loads.
> However, the user must install external protections corresponding to this type of loads (RC
> network, varistor, for inductive loads in AC; freewheel diode for inductive loads in DC).
