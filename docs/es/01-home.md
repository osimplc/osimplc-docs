
Open Simple Industrial Modular Programmable Logic Controller
==========

OSIMPLC es un Controlador Lógico Programable de arquitectura abierta (Open Source
Hardware) desarrollado en Argentina con propósitos educativos, y que puede ser aplicado a la
automatización de tareas de baja complejidad en entornos productivos industriales,
agropecuarios y comerciales, y en servicios residenciales, edilicios y urbanos.

## Open

Toda la información relacionada con el diseño de OSIMPLC (esquemáticos, listas de
materiales, posicionamiento de componentes, gráficos, tablas auxiliares, etc.) está
disponible para su descarga desde su sitio web, bajo licencia Creative Commons
Attribution-Share Alike, CC BY-SA, permitiendo su libre distribución, reutilización y
modificación.

Todos los softwares recomendados para la programación de OSIMPLC tienen licencia
libre GPL o compatible, y toda la documentación original referida a su utilización,
disponible en sus correspondientes sitios web, también es de licencia libre.

## Simple

El hardware de OSIMPLC ha sido desarrollado en base a circuitos integrados y a
componentes activos y pasivos disponibles habitualmente en el mercado local, y que
pueden ser fácilmente adquiridos en comercios minoristas y mayoristas de materiales
electrónicos; no requiere importación de componentes bajo demanda.
Los circuitos impresos que integran los diferentes módulos de OSIMPLC son de
tecnología de doble capa y agujero pasante metalizado (Two Layer Plated Through Hole
PCB); y pueden ser fabricados por la mayoría de las empresas del rubro sin complejos
requerimientos.

Ésto permite que OSIMPLC pueda ser construído, montado y/o reparado por el propio
implementador o el usuario, facilitando su aplicación en proyectos educativos donde
eventualmente puedan producirse daños por errores de conexión, y también su uso
entornos productivos, gracias a su menor costo relativo.

**OSIMPLC** ofrece las prestaciones básicas que se pueden encontrar en los PLCs de uso
industrial; está orientado a resolver tareas con requerimientos acotados de programa y
memoria, sin pretender competir con las funcionalidades avanzadas de dichos
equipamientos.

## Industrial

OSIMPLC está diseñado para interactuar con señales industriales normalizadas: trabaja
sólo bajo tensión de seguridad 24V (opcionalmente 12V para aplicaciones domóticas o
vehiculares); todas sus entradas y salidas digitales están optoacopladas; sus entradas
analógicas soportan señales estándar 0-10V, 0-5V, 0(4)-20mA y pueden también ser
configuradas para conectar directamente sondas PT100, NTC-10K y sensores LDR;
dispone de una salida analógica configurable en normas 0-10V/0(4)-20mA; ofrece un
puerto de comunicación con bus RS-485 half-duplex, y una conexión UART-TTL para
descarga del programa de usuario mediante adaptador USB/TTL y/o conexión de un
módulo HMI Serial TTL.

Las salidas digitales disponibles en los diferentes módulos cuentan con protecciones integradas: 
señales de control optoacopladas y limitadores de picos de tensión; y en los
módulos especializados de salidas, disponen de portafusibles individuales para cada
salida y para alimentación del módulo, haciendo innecesari el requerimiento de
portafusibles externos.

Todos los módulos de OSIMPLC cuentan con planos de masa diseñados para minimizar
los efectos de interferencias electromagnéticas, disponen de borneras para la conexión de
señales de campo (alimentación, entradas, salidas, comunicación), y soportan trabajar
entre 0 y 55 °C y hasta 95% de humedad sin condensación (condiciones estándar en la
mayoría de los gabinetes eléctricos industriales).

## Modular

El sistema OSIMPLC está basado en módulos que permiten configurarlo de acuerdo a las
tareas a automatizar, y a las señales de sensores y actuadores requeridas para cada
aplicación específica.

El módulo central [MCU](02-mcu.md) contiene a bordo el microcontrolador (en dos versiones); doce (12)
entradas digitales; doce (12) salidas digitales por optoacoplador (una de ellas configurable
como PWM); dos (2) entradas analógicas configurables; puerto RS-485; conector UART-
TTL; conector ICSP; conectores para módulos de expansión; entrada de alimentación y
salidas de tensión de referencia.

Los diferentes módulos de salidas (cada uno de ellos con cuatro puntos) pueden ser
fácilmente acoplados al módulo MCU, ofreciendo distintas combinaciones: relés;
transistores de potencia; mixto relés/transistor más salida analógica; directas a
optoacopladores más salida analógica (dedicado a variadores de frecuencia).

Los módulos de expansión a bordo agregan cuatro señales más, pudiéndose alcanzar
configuraciones máximas de hasta 30 Entradas/Salidas.

OSIMPLC dispone actualmente de ocho (8) diferentes módulos de expansión con distintas
configuraciones de E/S, algunos de ellos diseñados para resolver aplicaciones específicas
(detección de líquidos conductivos, sensado de temperaturas).

El [módulo HMI](05-hmi.md) ofrece una interfaz hombre/máquina simplificada; permite mostrar
mensajes de texto, variables, efectuar cambio de parámetros, facilitando la interacción del
usuario con el automatismo.

## Programmable Logic Controller

OSIMPLC puede ser fácilmente programado en lenguaje Ladder (IEC 61131-3), mediante
[LDmicro](07-ldmicro.md), software libre con licencia GPLv3. Ladder es el lenguaje utilizado más
habitualmente para la programación de PLCs industriales.

OSIMPLC también puede ser programado bajo [otros lenguajes](08-otherlangs.md) no incluídos bajo las
normativas IEC 61131-3, como BASIC, C, Assembler, utilizando software libre con licencia
GPL o compatible: GreatCowBASIC, SDCC + IDE, gputils + editor, u otros.

El código máquina compilado en un archivo .hex por LDmicro o por los otros softwares
libres disponibles, puede ser fácilmente descargado en OSIMPLC por medio del firmware
Tiny PIC Bootloader (preinstalado) y un conversor serial USB-TTL; o bien directamente
mediante un programador de PICs, utilizando los softwares de transferencia
correspondientes.

## OSIMPLC en proyectos educativos

Gracias a la implementación del lenguaje Ladder (IEC61131-3), al soporte de señales estándar
en industria, y a la facilidad de su construcción y/o reparación, OSIMPLC resulta un
equipamiento ideal para la enseñanza de la Automatización Programada en Escuelas de
Educación Técnica y en Centros de Formación Profesional, en las e specialidades Electricidad,
Electromecánica y Electrónica.

Las actividades educativas pueden abarcar desde el montaje de los componentes electrónicos
y la comprobación de su correcto funcionamiento (utilizando kits "para armar"), hasta el diseño,
la programación bajo Ladder y el montaje y puesta en funcionamiento de proyectos prácticos
complejos, que incluyan sensores industriales, elementos de maniobra habituales (relés
electromecánicos y de estado sólido, contactores, variadores de frecuencia, arrancadores
suaves), y los más diversos tipos de accionamientos (motores y motorreductores eléctricos,
electroválvulas y actuadores neumáticos y oleohidráulicos, resistencias calefactoras, etc. etc.).

De este modo, los educandos pueden integrar y articular la teoría y la práctica, posibilitando
una mejor transferencia de lo aprendido a diferentes contextos y situaciones de la actividad
productiva real.

## OSIMPLC en entornos productivos

OSIMPLC puede ser utilizado para implementar automatismos simples en:

* Maquinaria y líneas de producción de baja complejidad.
* Procesamiento de alimentos. Control complementario en cámaras frigoríficas, túneles de
congelado, autoclaves. Automatización de hornos, secaderos, germinadoras, lavadoras de
frutas y hortalizas, estufas de cultivo. Mezcladores, agitadores, amasadoras.
* Impulsión de agua potable, riego programado, ósmosis inversa, sanitizado de aguas.
Conducción y tratamiento de efluentes. Procesamiento de residuos.
* Ahorro de energía en calefacción, ventilación y aire acondicionado.
* Control de puertas de seguridad, portones, barreras, persianas y cortinas, rampas vehiculares.
* Automatización de montacargas, cintas transportadoras, tornillos sinfin.
* Iluminación en viviendas, edificios, comercios, industrias, estacionamientos. Alumbrado
público en calles, plazas, estadios, condominios, clubes.
* Señalización vial, semáforos, guía óptica/acústica de emergencia y/o evacuación.
* ... y miles de aplicaciones más!

