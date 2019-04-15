# MÓDULOS DE SALIDAS

Las salidas a señales de campo industriales de OSIMPLC están disponibles por medio de
módulos acoplables de cuatro salidas.

Dichas salidas son aptas para comandar actuadores o equipamientos de maniobra intermedios
que funcionen sólo bajo tensión de seguridad (24V), no estando permitido conectar elementos
que trabajen en tensión de red.

OSIMPLC MCU dispone de tres puertos, Y0-Y3, Y4-Y7 y Y8-YB/PWM, para conexión directa
de los módulos de salidas; algunos de éstos (sólo con salidas digitales) pueden ser conectados
indistintamente a cualquiera de los tres puertos, mientras que otros módulos (con salida
analógica o PWM) sólo pueden ser conectados al puerto Y8-YB/PWM.

Todos los módulos de salidas requieren de alimentación externa, disponen de fusible para su
circuito de control, y de fusibles de protección en cada una de sus salidas digitales (excepto en
módulo para control de variador de frecuencia), y en su salida analógica, en aquellos en que
está presente.

Las salidas digitales cuentan con protecciones limitadas contra sobretensiones en alimentación
y picos de tensión inversa originados por la desconexión de cargas inductivas.

No obstante, el usuario deberá instalar las protecciones externas corresponientes a este tipo de
cargas (red RC, varistor, para cargas inductivas en CA; diodo de de rueda libre para cargas
inductivas en CC).

## O4R: MÓDULO DE SALIDAS A RELÉS

Cuatro (4) salidas a relé, contacto NA, Unom: 24 Vca/cc; Imax: 2A carga resistiva, 1A
carga inductiva.

Alimentación del módulo: 24Vcc (estándar), 12Vcc (O4R-12, a pedido). Puede compartir la
alimentación con el módulo MCU, o utilizar fuente independiente.

> (imagen o4r)

## O4T: MÓDULO DE SALIDAS A TRANSISTORES DE POTENCIA

Cuatro (4) salidas a transistor tipo NPN colector abierto (MOSFET canal N), Unom: 24
Vcc; Imax: 2A carga resistiva, 1A carga inductiva.

Alimentación del módulo: 24V/12 Vcc, configurable por jumper. Puede compartir la
alimentación con el módulo MCU, o utilizar fuente independiente.

> (imagen o4r)

## OMX: MÓDULO DE SALIDAS MIXTO, RELEŚ + TRANSISTOR + SALIDA ANALÓGICA

Dos (2) salidas a relé, contacto NA, Unom: 24 Vca/cc; Imax: 2A carga resistiva, 1A carga
inductiva.

Una (1) salida a transistor NPN colector abierto, Unom: 24 Vcc; Imax: 200 mA carga
resistiva, 100 mA carga inductiva.

Una (1) salida analógica, configurable como 0-10V / 0(4)-20mA, resolución máx. 10 bits.

Alimentación del módulo: 24Vcc. Puede compartir la alimentación con el módulo MCU, o
utilizar fuente independiente.

> (imagen omx)

## OVF: MÓDULO PARA CONTROL DE VARIADOR DE FRECUENCIA

Tres (3) salidas para conexión directa de las de entradas digitales del variador de
frecuencia a los optoacopladores en el módulo MCU. Pueden utilizarse indistintamente
con señales PNP o NPN. No protegidas por fusible.

Una (1) salida analógica, normas 0-10V / 0(4)-20mA, resolución máx. 10 bits.

Alimentación del módulo: 24Vcc. Puede compartir la alimentación con el módulo MCU, o
utilizar fuente independiente.

> (imagen ovf)

## EN DESARROLLO

### ODCM: MÓDULO PARA CONTROL DE UN MOTOR EN CC

Salidas de Puente H, corriente hasta 10A en 24Vcc, más una (1) salida a transistor tipo
NPN colector abierto (MOSFET canal N), Unom: 24 Vcc; Imax: 2A carga resistiva, 1A
carga inductiva; inversión de rotación, control de velocidad por PWM, frenado/rueda libre
del motor, accionamiento de freno electromecánico.
