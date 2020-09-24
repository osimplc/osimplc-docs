# OSIMPLC-IDE Manual  

**OSIMPLC-IDE es el Entorno Integrado de Desarrollo que reúne todo el software necesario para programar OSIMPLC bajo el lenguaje Ladder, compilar la aplicación a un archivo .hex, y transferir el código máquina al microcontrolador por medio de comunicación serie, utilizando un conversor USB/UART.**  

**OSIMPLC-IDE, a partir de la v. 2.0.0, permite descargar directamente el código máquina en OSIMPLC desde el menú del programa LDmicro, sin requerir lanzar los programas de transferencia desde menús externos, archivos de escritorio o accesos directos.**  

**Los programas OBOOTLIN y TMBLDR+ para transferencia a microcontroladores PIC16F887 son versiones modificadas de Tiny Pic Bootloader for GNU/Linux y Tiny Multi Bootloader+, respectivamente; el programa OWL para transferencia a microcontroladores ATmega1284P no ha sido modificado**  
**Los archivos flashMcu_OSIMPLC_lin.bat y flashMcu_OSIMPLC_win.bat son para uso exclusivo en OSIMPLC; se distribuyen, al igual que el programa LDmicro, bajo licencia GPL3**  

---

### Noticia importante y advertencia:  

**OSIMPLC-IDE se encuentra bajo continuo desarrollo.**  

No existe una versión "estable"; sin embargo todas las versiones liberadas son plenamente funcionales.  
Consulte acerca de las características y compatibilidad de las nuevas versiones, o resuelva dudas o inconvenientes en instalación, enviando un email a info@osimplc.com .  

---

### Versiones:  

**v. 2.0.0**  

Agregado:  

1. Subdirectorio Programs > Templates: plantillas preconfiguradas para la programación bajo LDmicro con distintos microcontroladores (PIC16F887, ATmega1284P) y diferentes módulos de expansión.  
Configuración por defecto: PLC Cycle Time: 10 ms, MCU Crystal Frequency: 20 MHz, UART Baud Rate: 9600 bps.  

2. Archivos flashMcu_OSIMPLC_lin.bat y flashMcu_OSIMPLC_win.bat para automatizar la transferencia del código máquina al microcontrolador, desde el submenú Compile > Call flashMcu de LDmicro.  
Los programas OBootLin, TinyMultiBootloader+ o OWL son invocados automáticamente de acuerdo a la plantilla utilizada, o a la configuración establecida desde el menú Settings > Microcontroller de LDmicro.  

3. TMBLDR+: Directorio con software para S.O. MS Windows para transferencia del código máquina a microcontrolador PIC16F887. Definiciones para microcontroladores PIC16F887 y PIC18F4520 en archivo piccodes.ini del directorio TMBLDR+.  

4. Icono OSIMPLC-IDE.png  

Modificado:  

1. Nombres de directorios: AVRbldr -> OWL; PICbldr -> OBOOTLIN.  

**v. 1.0.0**  

Versión original de la IDE, sin automatización de transferencia del código máquina al microcontrolador desde el menú Compile > Call flashMcu de LDmicro.  

---

## INSTALACION  

## Requisitos previos:  

### Linux:  

1. Instale el programa wine https://www.winehq.org/ provisto por su distribución. Nota: Si su S.O. es de 64 bits, para instalar wine se le requerirá que habilite los repositorios Multiarquitectura y que actualice la base de datos de su sistema de paquetes. Siga las instrucciones de su distribución para instalar wine en su sistema.  
    1. Cree y configure el prefijo por defecto de wine, con arquitectura 32 bits: Desde una terminal, ejecute: `$ WINEARCH=32 WINEPREFIX=~/.wine winecfg`  
    1. En la ventana de Configuración de wine, pestaña Gráficos, marque las opciones Capturar automáticamente el puntero en las ventanas maximizadas (full screen), Permitir que el gestor de ventanas decore las ventanas y Permitir que el gestor de ventanas controle las ventanas. Desmarque la opción Emular un escritorio virtual; de acuerdo al tipo de entorno de escritorio que utilice, puede probar habilitar o deshabilitar algunas de ellas (verifique que los cuadros de diálogo de LDmicro no queden ocultos por otras ventanas).  

2. Instale las dependencias requeridas para ejecutar OBOOTLIN:  
    1. Debian, Ubuntu y derivadas: python3 python3-serial python3-wxgtk4.0  
    2. Arch Linux: python python-pyserial python-wxpython  
    3. Fedora: python3 python3-pyserial python3-wxpython4  

### Windows:  

1. Instale los runtime redistribuíbles de Visual C 2010 requeridos para ejecutar TMBLDR+ en la arquitectura de su sistema operativo,  
    - 32 bits: https://download.microsoft.com/download/5/B/C/5BC5DBB3-652D-4DCE-B14A-475AB85EEF6E/vcredist_x86.exe  
    - 64 bits: https://download.microsoft.com/download/3/2/2/3224B87F-CFA0-4E70-BDA3-3DE650EFEBA5/vcredist_x64.exe  

## Instalación de OSIMPLC-IDE  

1. Descargue el archivo OSIMPLC-IDE.zip.  
> https://github.com/osimplc/IDE/releases/download/first/OSIMPLC_IDE.zip  
2. Descomprima el archivo .zip descargado en un directorio (carpeta) de su usuario.  

### Estructura de directorios:  

- OSIMPLC-IDE  
    - LDmicro: Versión reducida de LDmicro estable, sólo ejecutable ldmicro.exe, scripts .bat y archivos .txt con cambios, licencia y manual en inglés; no contiene el directorio original LIBRARIES_FOR.  
    - Manual_HowTo: Documentación para OSIMPLC: manual de LDmicro en español (.html); conexión de sensores y actuadores, direccionamiento de E/S, comunicación por ICSP/UART,comandos para HMI (.pdf).  
    - OBOOTLIN: bootloader para transferencia de código máquina a OSIMPLC con microcontrolador PIC16F887, por comunicación serie y conversor USB/UART, para uso bajo S.O. GNU/Linux.  
    - OWL: bootloader para transferencia de código máquina a OSIMPLC con microcontrolador ATmega1284P, por comunicación serie y conversor USB/UART, para uso bajo S.O. GNU/Linux y MS Windows.  
    - Programs: Directorio para programas de usuario (aplicaciones): Guardar los archivos .ld en este directorio habilita operaciones de Copiar y Pegar entre diferentes programas.  
        - Templates: Plantillas preconfiguradas de programas para OSIMPLC con direcciones preasignadas para puertos para E/S digitales, entradas analógicas, salida PWM, puerto serie UART Tx, Rx, Dir.  
    - TMBLDR+:  bootloader para transferencia de código máquina a OSIMPLC con microcontrolador PIC16F887, por conversor serie USB/UART, para uso bajo S.O. MS Windows.  

## Configuración de OSIMPLC-IDE  

### Linux:  
1. Mediante un editor de textos copie el contenido del archivo /OSIMPLC-IDE/LDmicro/flashMCU_OSIMPLC_lin.bat en el archivo vacío /OSIMPLC-IDE/LDmicro/flashMCU.bat y guárdelo. Opcionalmente, puede renombrar directamente el archivo flashMCU_OSIMPLC_lin.bat como flashMCU.bat.  
2. Cree un lanzador de la IDE en el directorio de su preferencia (puede ser el Escritorio) copiando el archivo OSIMPLC-IDE.desktop, y edítelo adecuando las rutas al directorio donde ha instalado la IDE.

### Windows:  
1. Mediante un editor de textos copie el contenido del archivo \OSIMPLC-IDE\LDmicr\/flashMCU_OSIMPLC_win.bat en el archivo vacío \OSIMPLC-IDE\LDmicro\flashMCU.bat y guárdelo. Opcionalmente, puede renombrar directamente el archivo flashMCU_OSIMPLC_win.bat como flashMCU.bat.  
2. Mediante el administrador de archivos de Windows, cree un acceso directo al programa ldmicro.exe en el directorio de su preferencia (puede ser el Escritorio).  

---

## Uso de OSIMPLC-IDE  

**OSIMPLC-IDE facilita la creación de nuevas aplicaciones de usuario gracias a las plantillas preconfiguradas en el directorio Templates.**  

Estas plantillas de sólo lectura contienen las configuraciones para las diferentes combinaciones de MCU y módulos de expansión, con diferentes distribuciones de entradas digitales y analógicas, salidas digitales y por PWM, tanto para el MCU con microcontrolador PIC16F887 (estándar) como para el MCU con módulo eBrain y microcontrolador ATmega1284P (opcional).  

En dichas plantillas ya se ha programado la lectura de entradas, la escritura de salidas y la gestión de comunicaciones, de modo que la ejecución de la aplicación cumpla con la secuencia estándar del ciclo del PLC.  

También están preestablecidas las siguientes configuraciones por defecto en el PLC:  

1. Familia y modelo de microcontrolador: PIC16F887 (estándar), ATmega1284P (módulo eBrain), NO DEBE SER MODIFICADA.  
2. Tiempo de ciclo: establecido en 10 ms, puede ser modificado de acuerdo a necesidades específicas de la aplicación.  
3. Frecuencia de cristal: establecida en 20 MHz, NO DEBE SER MODIFICADA.  
4. Comunicación UART: establecida en 9600 baudios, puede ser modificada de acuerdo a necesidades específicas de la aplicación.  
5. Resistores pull-up: deshabilitados, NO DEBE SER MODIFICADA.  

OSIMPLC-IDE contiene también los scripts que permiten descargar directamente en el PLC la aplicación de usuario previamente compilada, por medio del conversor USB-TTL y el puerto UART del MCU.  
Estos scripts habilitan el menú Compile > flashMcu (F9) de LDmicro, llamando en forma automática a los programas bootloader correspondientes en función del microcontrolador a programar y del sistema operativo utilizado (ver arriba  Configuración de OSIMPLC-IDE).  

### Creación de una nueva aplicación.

1. Establezca la configuración de hardware de su aplicación: tipo de microcontrolador, módulos de salidas (sólo digitales ó mixtas con salida analógica controlada por PWM), módulos de expansión.
2. Ejecute el programa LDmicro, lanzándolo desde su archivo de escritorio, acceso directo, o mediante doble click en su archivo ejecutable.  
3. **Seleccione el menú File > Open, y mediante el cuadro de diálogo navegue hasta el directorio OSIMPLC > Programs > Templates.**  
4. **Seleccione y abra la plantilla que se corresponda con la configuración de hardware de su aplicación.**  
5. **Una vez abierta la plantilla corrrespondiente, SIN MODIFICARLA, mediante el menú File > Save as... guárdela como un archivo nuevo en el directorio superior Programs, con un nombre descriptivo adecuado a su aplicación.** Al guardar su aplicación en el directorio Programs, habilitará a OSIMPLC-IDE a compilar el archivo .hex en ese mismo directorio, y permitirá la descarga del código-máquina en OSIMPLC mediante los scripts que llaman a los programas de transferencia correspondientes desde el menú Compile > flashMcu (F9), sin necesidad de ejecutar dichos programas manualmente.  

### Edición de la nueva aplicación  

Al editar el nuevo archivo creado por el procedimiento anterior, tenga en muy consideración las siguientes indicaciones:  

1. Ingrese las instrucciones de su aplicación en la línea (escalón) libre sin completar, que encontrará bajo el siguiente comentario: **es: Inicio de la aplicacion de usuario. Agregue instrucciones en el escalon vacio, y nuevas lineas (escalones) a continuacion. en: User application beginning. Add instructions into empty rung, and new lines (rungs) after it.**  
2. NO MODIFIQUE la designación de entradas digitales (X0-Xn), entradas analógicas (A0-An), salidas digitales (Y0-Yn), salida PWMout, ni de los pines UART (Rx, Tx, Ydir).
3. Para facilitar la comprensión de su aplicación, Ud. puede modificar a su elección los nombres de los relés internos que guardan los estados leídos en las entradas analógicas (Rx0-Rxn, imagen de proceso de las entradas), los estados a escribir en las salidas digitales (Ry0-Ryn, imagen de proceso de las salidas), la dirección de comunicación en el puerto RS-485 half duplex (Rdir) y eventualmente el nombre de la variable duty_cycle que controla la salida PWM.  
4. Al desarrollar su aplicación, preste mucha atención a la consistencia en los nombres de las variables: relés internos, variables de temporizadores, contadores, etc., variables numéricas intermedias, y demás objetos programados. Recuerde que al tratar las diferentes variables, LDmicro es case-sensitive (diferencia minúsculas de mayúsculas), que los nombres de las variables NO pueden comenzar con un número, y que sólo pueden contener letras, números y el carácter subrayado (lea el Manual de LDmicro para más información sobre variables).  
5. Guarde el archivo de su aplicación mediante el menú File > Save (Ctrl + S, F2), opcionalmente puede guardar sus variantes mediante el menú File > Save as...  

### Compilación a archivo .hex.

Una vez editada su aplicación, debe compilar la misma en un archivo .hex para permitir que luego pueda descargarse el código-máquina en el microcontrolador, por medio del bootloader correspondiente.  

1. Mediante el menú Settings > MCU Parameters... confirme o modifique PLC Cycle Time (iempo de ejecución de lciclo de PLC) y UART Baud Rate (configuración de comunicaciones seriales) de acuerdo a los requerimientos de la aplicación.  
2. Verifique la correcta asignación de los nombres de las diferentes variables, y compruebe en el Panel de variables que se muestra en la zona inferior de la ventana de LDmicro, que todos los pines de E/S y comunicaciones han sido asignados.  
3. Compile el programa mediante el menú Compile > Compile (F5). El archivo generado será guardado en el mismo directorio con el mismo nombre que la aplicación y la extensión .hex.
4. Opcionalmente puede crear diversas compilaciones provisorias conteniendo modificaciones de la aplicación original mediante el menú Compile > Compile as..., sin necesidad  de cambiar el nombre original de la aplicación.  
5. Las otras opciones de compilación disponibles en el menú Compile no son utilizadas en OSIMPLC-IDE. Puede consultar sus características en el manual de LDmicro.  

### Transferencia del código-máquina al microcontrolador.  

Una vez compilada su aplicación en un archivo .hex, mediante OSIMPLC-IDE podrá descargar el código-máquina en el microcontrolador sin necesidad de lanzar manualmente los programas de transferencia (bootloaders).  

1. Inserte el conversor USB-UART en un puerto USB de su computadora.  
2. Verifique que su sistema operativo ha asignado correctamente el puerto serie a su conversor. Linux: /dev/ttyUSB0. Windows: COM1. En caso de asignación a otros dispositivos o puertos, Ud. puede eventualmente modificar el script flashMcu.bat copiado en la Configuración de OSIMPLC-IDE arriba descripta, o comunicarse con el desarrollador para resolver el inconveniente.
3. Inserte el terminal socket del conversor USB-UART en el puerto UART de OSIMPLC.  
4. Sistema operativo GNU/Linux:
>1. Mediante el menú Compile > flashMCU, inicie la transferencia del código-máquina. El archivo de compilación .hex a ser transferido es el que contiene el mismo nombre de la aplicación que está abierta en LDmicro. La IDE seleccionará automáticamente el programa de transferencia requerido para descargar el código-máquina en la variante específica del microcontrolador.  
>2. Antes de iniciar el proceso de descarga, un cuadro de diálogo le solicitará que presione y suelte rápidamente el botón RESET en OSIMPLC (esta acción activará el bootloader instalado en el microcontrolador). Acepte ese cuadro de diálogo mediante su botón OK y luego ejecute inmediatamente la acción solicitada.  
>3. Se cerrará el cuadro de diálogo e inmediatamente el programa de transferencia se comunicará con el bootloader instalado en el microcontrolador, iniciando la descarga del código-máquina. Este proceso podrá visualizarse por la actividad en los LEDs Tx y RX del conversor USB-UART.  
>4. NOTA: hasta el presente, OSIMPLC-IDE no dispone de cuadro de diálogo ni barra de progreso para mostrar el proceso de transferencia, ni para el software de transferencia OBootLin (PIC16F887, estándar) ni para el software de transferencia OWL (ATmega1284P, módulo eBrain). Hasta que se libere una nueva versión de la IDE, se deberá comprobar el proceso de transferencia visualizando los LEDs Tx y Rx en el conversor USB-TTL, y luego verificando la ejecución de la aplicación en OSIMPLC. El script flashMcu.bat asigna por defecto como puerto de comunicaciones al dispositivo virtual /dev/ttyUSB0. Si Ud. encuentra dificultades para efectuar la transferencia, desde una terminal virtual verifique dicha asignación mediante el comando $ ls -l /dev | grep ttyUSB .  
>5. Alternativamente, para el microcontrolador PIC16F887 (estándar) puede lanzar manualmente el programa OBootLin mediante su administrador de archivos , navegando hasta el directorio /OSIMPLC-IDE/OBOOTLIN y ejecutando el archivo obootlin.py mediante doble click. En el panel izquierdo deberá asignar el puerto de comunicaciones en el campo Port (opcionalmente puede buscarlo clickeando el botón Search) y seleccionar 19200 (OBLIGTORIO) en la lista desplegable Comm. Luego deberá ingresar la ruta y nombre del archivo .hex a transferir: utilice el botón Browse ubicado en el extremo superior derecho para seleccionarlo. Puede comprobar la comunicación con el bootloader mediante click en el botón CheckPIC, y presionando y soltando inmediatamente el botón RESET en OSIMPLC. Por último, deberá transferir el código-máquina mediante click en el botón Write Flash, y presionando y soltando inmediatamente el botón RESET en OSIMPLC.h. El cuadro Messages informará del proceso de transferencia y los eventuales errores. NOTA: en la pestaña Terminal dispondrá de un terminal virtual para comunicación serial con OSIMPLC. El baudrate puede ser diferente al utilizado por el bootloader, y debe ser establecido en el mismo valor que el configurado en su aplicación. El formato de la comunicación es 8N1.  
5. Sistema operativo Windows:  
> 1. OSIMPLC-IDE llama automáticamente al programa de transferencia TMBLDR+. El archivo de compilación .hex a ser transferido es el que contiene el mismo nombre de la aplicación que está abierta en LDmicro.  
> 2. En el panel izquierdo deberá asignar el puerto de comunicaciones en el campo Selected Port (opcionalmente puede seleccionarlo en el desplegable Detected Ports) y seleccionar 19200 (OBLIGTORIO) en la lista desplegable Baud Rate. Si el archivo .hex a transferir no se cargó automáticamene en el campo Selected File, deberá ingresar manualmente la ruta y nombre del archivo .hex a transferir: utilice el botón Browse ubicado en el extremo superior derecho para seleccionarlo. En la pestaña Configuration, marque Manual en el cuadro Reset Type. Puede comprobar la comunicación con el bootloader mediante click en el botón Check Device, y presionando y soltando inmediatamente el botón RESET en OSIMPLC. Por último, deberá transferir el código-máquina mediante click en el botón Write Device, y presionando y soltando inmediatamente el botón RESET en OSIMPLC.h. El cuadro Messages informará del proceso de transferencia y los eventuales errores. NOTA: TBLDR+ no dispone de terminal virtual para comunicación serial con OSIMPLC.  

6. NOTA: el software de transferencia OWL para el microcontrolador ATmega1284P, presente en el módulo de expansión eBrain, sólo se ejecuta en línea de comandos. Si ha instalado dicho módulo en OSIMPLC, y utilizando el menú Compile > flashMcu (F9) no puede completar la transferencia del código-máquina desde el archivo .hex compilado, por favor comuníquese con el desarrollador de OSIMPLC.

---

### Dudas, inconvenientes, resolución de problemas, sugerencias, colaboraciones:  

> **info@osimplc.com**

---
Daniel H. Mirkin  
Licencia Creative Commons Attribution ShareAlike - CC BY-SA  
