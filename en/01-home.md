Open Simple Industrial Modular Programmable Logic Controller
==========

OSIMPLC is an Open Source Hardware Programmable Logic Controller developed for
educative purposes in Argentina, it can be used for automation of low complexity tasks at
industrial, agricultural and big stores productive environments, and for house, buildings and
urban services.

## Open

All the information related to OSIMPLC design (schematics, bill of materials, components
positioning, graphics, auxiliary tables, etc.) is available to download from the web site
under Creative Commons Attribution-Share Alike license, CC BY-SA, allowing free
distribution, reuse and modification.

All the recommended softwares for OSIMPLC programming have free or GPL compatible
license, and all the original documentation referring on use is available on its
corresponding websites, also under free license.

## Simple

The OSIMPLC hardware has been developed based on integrated circuits and active and
passive components usually available in the local market, can be easily acquired in retail
stores and wholesalers of electronic materials; not on demand components import is
required.

The printed circuits that integrate the different modules of OSIMPLC are Two Layer Plated
Through Hole PCB; and can be manufactured by the majority of companies in the industry,
without complex requirements.

This allows OSIMPLC to be built, assembled and / or repaired by the implementer or the
user, facilitating its use in educational projects where damage may eventually occur due to
connection errors, and also its use in productive environments, thanks to its relative lower
cost.

**OSIMPLC** offers the basic features that can be found in PLCs for industrial use; is oriented
to solve tasks of low complexity or with limited requirements of program and memory,
without pretending to compete against the advanced functionalities of said equipments.

## Industrial

OSIMPLC is designed to interact with standard industrial signals: it works only under 24V
safety voltage (optionally 12V for home automation or vehicular applications); all its digital
inputs and outputs are opto-coupled; its analog inputs support 0-10V, 0-5V, 0(4) -20mA
standard signals and can also be configured to directly connect PT100, NTC-10K and LDR
sensors; has an analog output configurable in 0-10V / 0(4) -20mA standards; offers a
communication port with half-duplex RS-485 bus, and a UART-TTL connection for
downloading the user program via USB / TTL adapter and / or connection of a Serial TTL
HMI module.

The digital outputs available in the different modules have integrated protections: opto-
coupled control signals and voltage peak limiters; and in the specialized output modules,

they have individual fuse holders for each output and for feeding the module, making
unnecessary the requirement of external fuse holders.

All OSIMPLC modules have ground plans designed to minimize the effects of

electromagnetic interference, screw terminals for the connection of field signals (power,
inputs, outputs, communication), and support working between 0 and 55 ° C and up to
95% moisture without condensation (standard conditions in most industrial electrical
cabinets).

## Modular

The OSIMPLC system is based on modules that allow to configure it according to the
tasks to be automated, and the sensor and actuator signals required for each specific
application.

The [MCU](02-mcu.md) central module contains on board the microcontroller in two versions; twelve (12)
digital inputs; twelve (12) opto-coupled digital outputs (one of them configurable as PWM);
two (2) configurable analog inputs; RS-485 port; UART-TTL connector; ICSP connector;
expansion modules connectors; power input and reference voltage outputs.

The different output modules (each with four points) can be easily coupled to the MCU
module, offering different combinations: relays; power transistors; mixed relays / transistor
plus analog output; optocouplers plus analog output, dedicated to frequency inverters.

On board expansion modules add four more signals, allowing to reach configurations of up
to 30 Inputs / Outputs max.

OSIMPLC currently has eight (8) different expansion modules with different I/O
configurations, some of them designed to solve specific applications (conductive liquid
detection, temperature sensing).

[HMI module](05-hmi.md) offers a simplified human/machine interface; allows to display text messages,
variables, change in parameters, facilitating user interaction with the automatism.

## Programmable Logic Controller

OSIMPLC can be easily programmed in Ladder language (IEC 61131-3) through [LDmicro](07-ldmicro.md),
free software with GPLv3 license. Ladder is the language most commonly used for
industrial PLCs programming.

OSIMPLC can also be programmed using [other languages](08-otherlangs.md) not included under IEC 61131-
3 normatives, such as BASIC, C, Assembler, using free software with GPL or compatible
license: GreatCowBASIC, SDCC + IDE, gputils + editor, or others.

The machine code compiled in a .hex file by LDmicro or by the other available free
softwares, can be easily downloaded in OSIMPLC by means of the Tiny PIC Bootloader
firmware (pre-installed) and a USB-TTL serial converter; or directly through a PIC
programmer, using the corresponding transferring software.

## OSIMPLC in educational projects

Thanks to the implementation of the Ladder language (IEC61131-3), industrial standard
signals support , and ease construction and/or repairing, OSIMPLC is an ideal equipment
for teaching Programmed Automation in Technical Education Schools and in Professional
Training Centers, in Electricity, Electromechanics and Electronics specialties.

The educational activities can range from the assembly of the electronic components and
the verification of their correct operation (using "to assemble" kits), to the design,
programming under Ladder, and the assembly and implementation of complex practical
projects, including industrial sensors, common operating elements (electromechanical and
solid-state relays, contactors, frequency inverters, soft starters), and the most diverse
types of drives (motors and electric gearmotors, electro-valves and pneumatic and

oleohydraulic actuators, heating resistors, etc.). etc.).

In this way, students can integrate and articulate theory and practice, enabling a better
transfer of what is learned to different contexts and situations of real productive activity.

## OSIMPLC in productive environments

OSIMPLC can be used to implement simple automations in:
* Machinery and production lines of low complexity.
* Food processing. Complementary control in cold rooms, frozen tunnels, autoclaves.
Automation for ovens, dryers, germinators, fruit and vegetable washers, cultivation stoves.
Mixers, stirrers, kneading machines.
* Drinking water supply, programmed irrigation, reverse osmosis, water sanitization.
Conduction and treatment of effluents. Waste processing.
* Energy saving in heating, ventilation and air conditioning.
. Control of security doors, gates, barriers, louvers and courtains, vehicular ramps.
* Automation of lifttrucks, conveyor belts, screw conveyors.
* Lighting in homes, buildings, shops, industries, parking lots. Public lighting in streets,
squares, stadiums, condominiums, clubs.
* Road signalization, traffic lights, optical/acoustic emergency and/or evacuation guide.
* ... and thousands of other applications!
