# OBootLin

OBootLin es un programa que permite descargar en la memoria Flash de OSIMPLC el archivo \*.hex (INTEL IHEX) que contiene el código máquina compilado por LDmicro a partir de un esquema Ladder (archivo \*.ld), o generado por medio de otros softwares de programación y sus compiladores.
OBootLin corre bajo sistemas operativos GNU/Linux, y tiene licencia GPLv2.

OBootLin es una versión ligeramente modificada de TinyBootloaderLin, y utiliza firmwares adaptados para los microcontroladores preinstalados en OSIMPLC: PIC16F887 (estándar) y PIC18F4520 (alternativo), con oscilador por cristal externo a 20 MHz.

Estos firmwares son totalmente compatibles con otros softwares libres y privativos que pueden realizar la misma tarea, todos ellos derivados del original Tiny Pic Bootloader TPBL: TinyBootloaderLin (original y fork), bajo S.O. GNU/Linux; TPBLwin y MultiBootloader+, bajo S.O. Windows.

---

## Instalar OBootLin

### Descargar OBootLin:

Desde [OSIMPLC página Descargas](http://www.osimplc.com/Downloads/), descargue el archivo OBootLin.zip

### Dependencias requeridas

#### Arch Linux:
`python python-pyserial python-wxpython`

#### Debian, Ubuntu y derivadas

`python3 python3-serial python3-wxgtk4.0`

#### Fedora:

`python3 python3-pyserial python3-wxpython4`

### Instalación

No se requiere ningún proceso especial de instalación.
Simplemente, descomprimir el archivo OBootLin.zip en un nuevo subdirectorio dentro del directorio /home/$USER, preferentemente junto al subdirectorio donde se instaló LDmicro.

---

## Usar OBootLin

### Comunicar OSIMPLC con la PC:

1. Conectar el Cable de Programación de OSIMPLC, o un adaptador USB-TTL genérico, al puerto USB en la PC y al conector TTL (pinout) en OSIMPLC.
Bajo GNU/Linux, el cable de programación o el adaptador aparecerán como un dispositivo /dev/ttyUSB[0-9].

2. Comprobar la asignación de puerto desde una terminal virtual:
```
[$USER@$hostname ~]$ ls -l /dev | grep ttyUSB
crw-rw----  1 root uucp    188,   0 mar  2 13:13 ttyUSB0
```

> **Nota**: si otro adaptador USB-serial ha sido conectado previamente, el cable de programación o nuevo adaptador USB-serial genérico será renumerado como /dev/ttyUSB1, y así sucesivamente.

**IMPORTANTE:**
El usuario debe ser agregado al grupo que permite acceso a los dispositivos USB-serial (se requieren permisos de root):

Arch Linux y otras (Fedora, openSUSE): grupo `uucp`.
Debian, Ubuntu y derivativas: grupos `dialout` y `plugdev`.

### Ejecutar OBootLin:

1. En un terminal virtual, navegar al subdirectorio OBootLin: `[$USER@$hostname ~]$ cd /home/$USER/../OBootLin/`
2. Ejecutar OBootLin mediante python3: `[$USER@$hostname ObootLin]$ python3 obootlin.py`
3. Alternativamente, se puede generar un lanzador OBootLin.desktop que permitirá ejecutar automáticamente OBootLin, sin necesidad de hacerlo desde una terminal virtual. Se pueden instalar copias del archivo OBootLin.desktop en diferentes directorios personales: Escritorio (Desktop) y/u otros. Utilizar un editor de texto para copiar y modificar el siguiente ejemplo de lanzador: *OBootLin.desktop*
```
     [Desktop Entry]
     Name=OBootLin
     Exec=/home/$USER/ruta/al/directorio/OBootLin/obootlin.py
     Type=Application
     StartupNotify=true
     Path=/home/$USER/ruta/al/directorio/OBootLin/
     Icon=/home/$USER/ruta/al/directorio/OBootLin/modules/images/obootlin.png
```

### Configurar el puerto y baudrate en la interfaz gŕafica de OBootLin:

En la sección de configuración de OBootLin (izquierda abajo), configurar:

1. Velocidad de transmisión (baudrate): en la lista desplegable Comm, seleccione 19200 (obootlin.py utiliza 19200 baudios por defecto para la programación y la comprobación).
2. Búsqueda de puerto: presione el botón Search, el campo de texto Comm mostrará el puerto virtual /dev/ttyUSB[0-9] asignado al cable de programación o al adaptador USB-TTL genérico. En caso de error, mostrará el mensaje "No serial ports detected".
3. Campo de texto Comm: alternativamente, se puede establecer manualmente el puerto /dev/ttyUSB[0-9], de acuerdo a la asignación del S.O. (ver arriba: comprobar la asignación de puerto).
4. Es conveniente marcar la opción Check File, para evitar transferir programas corruptos o inexistentes.
5. Si se utiliza el cable de programación de OSIMPLC o un adaptador USB-TTL genérico, no es necesario marcar la opción RTS (RequestToSend); ést0 sólo podría ser requerido para realizar la comunicación por puerto serie nativo DB9 ttyS[0-3], actualmente obsoleto.

### Comprobar la comunicación e identificación de OSIMPLC:

1. En la sección de comando de OBootLin (izquierda arriba), presionar el botón CheckPIC y mientras la barra de progreso está avanzando (timeout), en OSIMPLC presionar y soltar inmediatamente el botón RESET.

2. Si la operación resulta existosa, la pestaña Messages mostrará:
 >Connected to /dev/ttyUSB0 at 19200
  Searching for PIC ...
  Found:16F 886/887 [18F 452o]

3. En caso de error, mostrará el mensaje: "Could not connect to ttyUSB0 at 19200 ERROR!" si no puede establecer la comunicación con el cable de programación o adaptador USB-TTL;
o el mensaje: "Connected to /dev/ttyUSB0 at 19200 Searching for PIC... Not found ERROR!" si OSIMPLC no tiene alimentación, se encuentra aún reseteado o existe algún otro inconveniente.

### Transferir el programa de usuario (compilado a código máquina) a OSIMPLC:

1. Seleccionar el archivo *.hex a transferir (compilado por LDmicro u otro software) utilizando el botón Browse (arriba a la derecha). Se abrirá el cuadro de diálogo SelectHexFile que permite navegar por directorios y subdirectorios, y seleccionar el archivo que contiene el correspondiente programa de usuario.

2. En la sección de comando de OBootLin (izquierda arriba) presionar el botón Write Flash, y mientras la barra de progreso está avanzando (timeout), en OSIMPLC presionar y soltar inmediatamente el botón RESET.

3. Si la operación resulta exitosa, la pestaña Messages mostrará:
 >Connected to /dev/ttyUSB0 at 19200
  HEX:xx days old,INX32M,16Fcode+cfg,total=xxxx bytes.
  Searching for PIC ...
  Found:16F 886/887
  Write OK at hh:mm time: x.xxx sec

4. En caso de error, mostrará el mensaje: "Could not connect to ttyUSB0 at 19200 ERROR!" si no puede establecer la comunicación con el cable de programación o adaptador USB-TTL;
o el mensaje: "Connected to /dev/ttyUSB0 at 19200 HEX:xx days old,INX32M,16Fcode+cfg,total=xxx bytes.  Searching for PIC ...Not found", si OSIMPLC no tiene alimentación, se encuentra aún reseteado o existe algún otro inconveniente.

### Enviar y recibir datos del OSIMPLC por medio del terminal serial virtual incluído:

**NOTA:
Para efectuar la comunicación entre OSIMPLC y la PC, puede utilizarse el cable de programación, un adaptador USB-TTL o un adaptador USB-RS485.**

1. En la pestaña Terminal: seleccionar la velocidad de transmisión (baudrate) utilizando la lista desplegable.

>NOTA: la velocidad de transmisión (baudrate) de datos mediante UART en el programa de usuario puede ser diferente de la velocidad utilizada por OSIMPLC para programación y comprobación (19200), y debe ser definida en el menú MCU parameters de LDmicro (o en el menú/configuración correspondiente en otros softwares).

2. Iniciar la comunicación: presionar el botón Open.

3. Recibir datos:

Si OSIMPLC está enviando datos por su puerto serie (TTL o RS-485), éstos serán mostrados en el campo de texto.
Utilizando la lista desplegable Rx, el usuario puede ver los datos como Char (carácter) o Hex (códigos hexadecimales).
Mientras el puerto de comunicaciones permanece abierto, el usuario puede seleccionar y copiar el contenido del campo de texto (datos enviados por OSIMPLC).

4. Enviar datos:

Utilizando la lista desplegable Tx, el usuario puede enviar datos como Char (carácter).
El usuario puede tipear los datos (caracteres) en el campo de texto Tx, o copiarlos desde un archivo y pegarlos en dicho campo.
Luego, debe presionar el botón Send, o la tecla Enter en el teclado para ejecutar el envío.
Otros modos de datos (char\, Type, TypEcho) pueden ser también utilizados.

5. Finalizar la comunicación: presionar el botón Close.

---

## Historia del Software

### OBootLin

www.osimplc.com/Downloads/

Este código derivado es una versión ligeramente modificada de Tiny Pic Bootloader for GNU/Linux, Luis Claudio Gamboa fork.

El módulo pictype.py ha sido modificado, sólo contiene identificadores para los microcontroladores PIC16F887 y 18F4520.
Además, sólo contiene firmware (archivos .hex) para los microcontroladores PIC16F887 y 18F4520 con oscilador por cristal externo a 20 MHz.

### Tiny Pic Bootloader for GNU/Linux, Luis Claudio Gamboa fork

https://github.com/lcgamboa/tinybldlin

Este código derivado es una versión modificada para permitir diferentes tamaños de bootloader (firmware), definidos por el usuario en el módulo "modules/pictype.py".
Además, el código original ha sido convertido para ser utilizado con el intérprete de python3 (a partir del año 2020, python2 quedará obsoleto y no dispondrá de actualizaciones ni soporte).

Última versión: d7cf21b, 16 Diciembre 2018.

### Tiny Pic Bootloader for GNU/Linux

http://tinybldlin.sourceforge.net/

El software Tiny Pic Bootloader for GNU/Linux original fue escrito por Fernando Juarez V.
Tiny Pic Bootloader for GNU/Linux es licenciado bajo GPL v2.

TinybldLin fue escrito en python2 utilizando los módulos python-serial y wxphython; puede ser ejecutado en cualquier distribución GNU/Linux (y quizás mac) que tenga instaladas dichas dependencias.
"Esta versión pretende ser más que un clon del tinybldWin.exe original; pretende, en el futuro, agregar nuevas características y mejorar las existentes en el original."

Última versión: tinybldlin-0.8.1-src.tar.gz, 15 Noviembre 2012.

### Tiny Pic Bootloader for Windows

http://www.etc.ugal.ro/cchiculita/software/picbootloader.htm

Tiny Pic Bootloader (tinybld) fue escrito por Claudiu Chiculita, quien desarrolló el firmware (bootloader) y las primeras versiones del software para Windows (bajo Delphi).

Los firmwares de tinybld son los bootloaders más pequeños disponibles para PICs, ocupan un máximo de 100 palabras (200 bytes) para aquellos dispositivos de las líneas 16F, 18F, dsPIC30 que soportan autoprogramación (self programming).

Tiny Pic Bootloader for Windows es un programa gratuito (freeware), pero no tiene una licencia de software libre, ni está disponible su código fuente.

Última versión: TinyBld-1_10_6_pc_beta.zip, 18 Junio 2011.












