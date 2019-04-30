# MÓDULO CENTRAL MCU: MICRO CONTROLLER UNIT

El módulo MCU es el núcleo de OSIMPLC; dispone de entradas integradas, conectores a
módulos de salidas, a módulos de expansión, y a diversos modos de comunicación y
programación.

El módulo MCU contiene a bordo el microcontrolador, en dos versiones fácilmente
intercambiables:

PIC16F887 (estándar): puede ser programando en lenguaje Ladder (IEC 61131-3)
mediante LDmicro, y también en lenguajes BASIC, C y Assembler; aplicable para
resolución de tareas simples;

PIC18F4520 (opcional): puede ser programando en lenguajes BASIC, C y Assembler
(LDmicro no soporta la línea PIC18F). Ofrece el doble de capacidad en memoria para
programa de usuario y el cuádruple de capacidad en memoria volátil (RAM) comparado
con el PIC16F887; aplicable para resolución de tareas relativamente complejas.

![MCU](images/mcu.png)

## Alimentación

Una (1) entrada para alimentación 24Vcc / 12Vcc (rango 11~27Vcc) con protección por
fusible y contra inversión de polaridad, señalizada por LED;

Tres (3) salidas de tensión de referencia: 0V; 2,5V (máx. 10mA); 5V (máx. 50mA); no
protegidas.

![MCU](images/power_reg.png)

## Entradas

Doce (12) entradas digitales optoacopladas tipo PNP (sink), en tres grupos de cuatro
entradas más borne de referencia externa (pueden utilizarse con fuentes de alimentación
independientes); señalizadas por LEDs;

![Digital inputs](images/digital_inputs.png)


Dos (2) entradas analógicas no aisladas, configurables por medio de jumpers en las
normas 0-10V, 0-5V ó 0(4)-20mA; una de ellas también configurable para conexión de una
sonda PT100 (provee excitación de 1 mA), la otra también configurable para conexión de
sensor de temperatura NTC-10K ó sensor LDR.

![Analog Inputs + Power](images/analog_inputs+power.png)


## Salidas

Doce (12) salidas digitales por optoacopladores, de muy baja corriente (máx. 15 mA), una
de ellas configurable como digital optoacoplada ó como PWM no aislada por medio de
jumpers, con conectores para los módulos especializados de salidas (tres grupos de
cuatro salidas); señalizadas por LEDs.

![Outputs](images/outputs.png)

## Expansión

Dos (2) conectores para módulos de expansión; uno para módulos a bordo (expansiones
estándar), y el otro para módulos externos diseñados por el propio usuario o bajo pedido.

![Conectores para módulos de expansión](images/mcu-expansions.png)

## Comunicación

Un (1) puerto RS-485 half-duplex, disponible para conexión con instrumental industrial,
otros controladores programables y/o PC, modo transmisión señalizado por LED;

Un (1) conector UART-TTL, disponible para descarga del programa de usuario mediante
bootloader (Tiny Pic Bootloader) y/o monitoreo desde una PC utilizando un conversor
USB-TTL, o para conexión del módulo HMI Serial TTL;

![TTL-RS485](images/ttl-rs485.png)

Un (1) conector ICSP (In Circuit System Programming) para instalación del bootloader, o
escritura y lectura del programa de usuario y/o modificación de configuraciones del
microcontrolador, utilizando programadores de PIC como USBPICPROG (Open Hardware + Free Software), PicKit2 u otros.

![ICSP](images/ICSP_reset.png)


La transferencia del código máquina al OSIMPLC puede ser realizada por medio de
comunicación serial utilizando un bootloader y un conversor USB-TTL conectado al pin-out
UART, o por transferencia directa utilizando un programador de PICs conectado al pin-out
ICSP.

> **Nota**:
> La utilización del conector ICSP es excluyente con la utilización de las salidas Y0-Y1; la
> utilización del conector UART-TTL es excluyente con la conexión del puerto RS-485.

#### OSIMPLC provee como accesorio un cable de programación USB-TTL
