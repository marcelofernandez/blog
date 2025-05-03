---
author: Marcelo
category:
  - codear
  - linux
  - ubuntu-ar
date: "2007-12-09T19:13:00+00:00"
guid: https://blog.marcelofernandez.info/?p=92
title: Ubuntu Live-USB Personalizada (Distro USB booteable)
url: /2007/12/ubuntu-live-usb-personalizada-distro-usb-booteable/

---
Hola!

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/R1w7JLd9BQI/AAAAAAAAAN4/J_DXvh19C_s/s200/Memoria_USB_sony_2.0_Vaio_512_mb.JPG)](http://4.bp.blogspot.com/_nDZ247g0qSM/R1w7JLd9BQI/AAAAAAAAAN4/J_DXvh19C_s/s1600-h/Memoria_USB_sony_2.0_Vaio_512_mb.JPG) Hace algún tiempo escribí esto y ahora lo voy a plasmar acá. Se trata de una guía para armar una distribución Linux ("customizada" o "personalizada", si se quiere) booteable desde un dispositivo USB como por ejemplo un [Pen Drive](http://es.wikipedia.org/wiki/Memoria_USB), lo que también se llama [Live-USB](http://en.wikipedia.org/wiki/Live_USB) (por su cercana relación con los [Live-CDs](http://es.wikipedia.org/wiki/LiveCD)).

Mi necesidad era crear un Live-CD bien genérico, con Linux, entorno gráfico (Gnome preferentemente) más una aplicación mía (que arrancara automáticamente) a una PC que no tenía lectora (ni podía tenerla) pero sí tenía USB; con lo que no me importaba si el acceso al dispositivo era de lectura/escritura o de lectura solamente, una vez arrancado Linux. Al final resultó que el acceso era de sólo lectura, pero calculo que googleando un poco o tocando el archivo de configuración del initramfs se puede hacer de lectura/escritura. También puede servir [este link](https://help.ubuntu.com/community/LiveCDPersistence), donde no se modifican las opciones "montaje" del dispositivo raíz, pero es una manera alternativa para salir del paso.

De más está decir, que para seguir esta "guía", el lector debe conocer los comandos de Linux y estar utilizándolo al momento de seguir estos pasos.

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/R1w7aLd9BRI/AAAAAAAAAOA/75uJO_jl3Og/s400/headerlogo.png)](https://help.ubuntu.com/) Basándonos en [éstas instrucciones del sitio de Ubuntu](https://help.ubuntu.com/community/LiveCDCustomizationFromScratch) para generar un LiveCD booteable personalizado de Ubuntu 7.04 Feisty Fawn, vamos a seguir dichas instrucciones y al final, en vez de generar una ISO para grabar en un CD, vamos a generar un Pen Drive booteable.

Requerimientos

\\* SO Linux para generar todo, permisos de root
\\* CD de Instalación de Ubuntu
\\* squashfs-tools
\\* qemu (opcional)

Pasos:1\. Crear una "jaula" Chroot e instalar los paquetes necesarios allí1.a. Instalar el paquete 'debootstrap'.

```
$ sudo -i (ingresamos nuestra contraseña para convertirnos en root).
# apt-get install debootstrap
```

1.b. Ejecutar:

```
# mkdir work
# cd work
# mkdir chroot
# debootstrap --arch i386 feisty chroot  http://archive.ubuntu.com/ubuntu
```

Esto va a crear un directorio de trabajo ('work') con un directorio chroot adentro. Vamos a instalar un pequeño Ubuntu allí.

```
# cp /etc/resolv.conf chroot/etc/resolv.conf
# cp /etc/apt/sources.list chroot/etc/apt/sources.list

# chroot chroot

# mount /proc
# mount /sys

# apt-get update
# locale-gen es_AR.UTF-8

# apt-get install ubuntu-standard casper
# apt-get install discover1 laptop-detect os-prober

# apt-get install linux-generic
# apt-get clean

# rm -rf /tmp/*
# rm /etc/resolv.conf

# umount /proc
# umount /sys

# exit
```

Lo que hacemos ahí es:

- Copiar el archivo para poder resolver peticiones DNS, las URLs de los repositorios de Ubuntu.
- Entramos al chroot.
- Luego montamos los dos filesystems 'lógicos' de nuestro Linux para poder instalar paquetes normalmente dentro del chroot.
- Dichos paquetes a instalar son los básicos para que se inicie el SO y se ejecute la consola (pueden instalarse los paquetes que se quieran, aunque como después es posible volver a entrar y hacer modificaciones, sugiero instalar sólo estos en primera instancia).
- Por último se eliminan archivos temporales, los paquetes .deb descargados y se trata de 'descontaminar' el chroot de los archivos generados y/o copiados anteriormente. Gran parte de esto se hace para que la jaula chroot sea lo más pequeña posible (aunque posteriormente se comprima).
- Con el comando 'exit' (o también con la combinación de teclas Ctrl+D), se sale del chroot.

Con todo esto lo que hacemos es construir nuestro sistema personalizado utilizando paquetes de los repositorios de Ubuntu.

2\. Crear el directorio 'image' del medio a bootear2.1. Instalar los paquetes 'squashfs-tools', 'syslinux', 'sbm'. El paquete squashfs-tools nos va a permitir comprimir (y montar si queremos) el filesystem que está reflejado en el chroot. Syslinux nos da las herramientas para poder hacer el pen drive booteable. SBM es una herramienta que es útil tener por si booteo no funciona.

2.2. Dentro del directorio de trabajo ('work'), crear el subdirectorio 'image' y el subdirectorio 'casper', donde va a residir el sistema de archivos que generamos en el chroot pero comprimido con [squashfs](http://en.wikipedia.org/wiki/Squashfs).

```
# mkdir image image/casper
```

2.3. Va a ser necesario un kernel y un initrd construído con los scripts de casper. Obténgalos de su chroot.

```
# cp chroot/boot/vmlinuz-2.6.20-16-generic image/casper/vmlinuz
# cp chroot/boot/initrd.img-2.6.20-16-generic image/casper/initrd.gz
```

2.4. También se necesitará el binario de sbm y el kernel de testeo de memoria (opcional). Los archivos los saco de la propia instalación de Ubuntu Feisty con la cual se está usando la PC, cuidado si no se está utilizando la misma distribución/versión...) en todo caso, si estoy usando otra versión de Ubuntu o Debian, podría instalar los paquetes en el chroot y copiarlos de ahí.

```
# cp /boot/memtest86+.bin image/install/memtest
# cp /boot/sbm.img image/install/
```

2.5. Cree un archivo syslinux.txt en el directorio 'image' para mostrar al bootear ('image' será la raíz del pen drive). Por ejemplo:

```
^Xsplash.rle

Para iniciar el sistema, presione ENTER.
```

^X es el caracter nro. 24 de la tabla ASCII (en la documentación de syslinux figura como <can>). Sirve para indicar que lea el archivo splash.rle (en este caso) y lo muestre modo gráfico en la pantalla. Para más detalles, como por ejemplo, generar una imagen de un formato soportado, ver la [documentación de syslinux](http://syslinux.zytor.com/faq.php).

2.6. Cree un archivo syslinux.cfg en el directorio 'image'. Puede leer la documentación de syslinux en /usr/share/doc/syslinux/syslinux.doc para ver las opciones de configuración. Aquí hay un ejemplo:

```
DEFAULT live
LABEL live
menu label ^Start or install Ubuntu
kernel vmlinuz
append  file=/cdrom/preseed/ubuntu.seed boot=casper initrd=initrd.gz vga=normal splash --
LABEL check
menu label ^Check CD for defects
kernel vmlinuz
append  boot=casper integrity-check initrd=initrd.gz splash --
LABEL memtest
menu label ^Memory test
kernel memtest
append -
LABEL hd
menu label ^Boot from first hard disk
localboot 0x80
append -
DISPLAY syslinux.txt
TIMEOUT 50
PROMPT 1
```

2.7. Cree el manifest (siempre posicionado en el directorio 'work').

```
# chroot chroot dpkg-query -W --showformat='${Package} ${Version}\n' > image/casper/filesystem.manifest
# cp image/casper/filesystem.manifest image/casper/filesystem.manifest-desktop
# sed -ie '/ubiquity/d' image/casper/filesystem.manifest-desktop
```

2.8. Comprima el chroot (posicionado en el directorio 'work')

```
# mksquashfs chroot image/casper/filesystem.squashfs
```

2.9. Cree un diskdefines (posicionado en el directorio 'work')

```
# vi image/README.diskdefines
```

que contenga, por ejemplo:

```
#define DISKNAME  Ubuntu 7.04 "Feisty Fawn" - Release i386 **Remix**
#define TYPE  binary
#define TYPEbinary  1
#define ARCH  i386
#define ARCHi386  1
#define DISKNUM  1
#define DISKNUM1  1
#define TOTALNUM  0
#define TOTALNUM0  1
```

2.10. Calcule la firma md5 de cada archivo y guárdela en la raíz de la imagen.

```
# (cd image && find . -type f -print0 | xargs -0 md5sum > md5sum.txt)
```

3\. Formatear el Pen Drive y copiar los archivos(!) Esta operación elimina completamente el contenido del pen drive.(!) Cuidado al utilizar estas instrucciones como usuario root y equivocarse de archivo en /dev/. Las máquinas más o menos actuales incorporan discos SATA, que el kernel de Linux las identifica como '/dev/sdx', de la misma forma que los pen drives. Si llegara a ejecutar un 'dd if=xxx of=/dev/sda', donde sda es el HD SATA, borrará todo el disco rígido.

Lo que puede hacer para averiguar qué /dev/sdX es su pen drive (donde X puede ser 'a','b','c', etc.), es insertarlo y ejecutar el comando 'dmesg' y ver las últimas líneas que automáticamente imprime algo como lo siguiente cuando se inserta un pen drive:

```
[186301.602058] usb 5-7: new high speed USB device using ehci_hcd and address 8
[186301.738659] usb 5-7: configuration #1 chosen from 1 choice
[186301.738915] scsi10 : SCSI emulation for USB Mass Storage devices
[186301.739126] usb-storage: device found at 8
[186301.739129] usb-storage: waiting for device to settle before scanning
[186306.735664] usb-storage: device scan complete
[186306.736286] scsi 10:0:0:0: Direct-Access     Kingston DataTraveler 2.0 1.00 PQ: 0 ANSI: 2
[186306.738254] SCSI device sdb: 1994752 512-byte hdwr sectors (1021 MB)
(sigue...)
```

En este caso, como se puede ver, el dispositio es sdb. Otra opción es, con el pen drive puesto, ir al menú "Sistema -> Preferencias -> Información de Hardware". En dicho programa buscar el pen drive (en mi caso "Data Traveler 2.0"), y en la pestaña avanzado, en la grilla de propiedades aparecerá primero la key "block.device" de tipo "string" y en el valor estará el nombre de /dev/sdX que lo representa:

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/R1w4xbd9BPI/AAAAAAAAANw/dL_tgZIe-sI/s400/Administrador_Dispositivos.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/R1w4xbd9BPI/AAAAAAAAANw/dL_tgZIe-sI/s1600-h/Administrador_Dispositivos.png)
Una vez que se averiguó el dispositivo que utiliza la PC para hacer referencia al Pen Drive, seguimos los siguientes pasos:

3.1 Borramos totalmente el primer sector (el [Master Boot Record](http://es.wikipedia.org/wiki/Master_Boot_Record)) del Pen Drive:

```
# dd if=/dev/zero of=/dev/sdX bs=512 count=1
```

(Donde /dev/sdX es el nombre de archivo que el SO usa como referencia al Pen Drive, /dev/sdb en mi caso). Esto se hace por única vez por cada pen drive.

3.2. Ejecuto el gestor de particiones, fdisk, sobre el pen drive. La idea es generar una única partición que ocupe la totalidad del tamaño del pen drive, de tipo FAT 16 y que tenga la marca de booteo.

```
# fdisk /dev/sdb

Orden (m para obtener ayuda): p

Disco /dev/sdb: 1021 MB, 1021313024 bytes
32 cabezas, 61 sectores/pista, 1021 cilindros
Unidades = cilindros de 1952 * 512 = 999424 bytes

Disposit. Inicio    Comienzo      Fin      Bloques  Id  Sistema
/dev/sdb1   *           1        1021      996465+   6  FAT16

Orden (m para obtener ayuda):
```

Con 'p' veo la estructura actual, y tecleo 'n' (de New Partition) para crear la nueva partición, seguido de un par de enters (uno para definir el inicio y otro el final; por defecto la nueva partición toma todo el espacio disponible). Una vez que volví al prompt del fdisk, tipeo 't' para cambiarle el tipo de partición, en donde elijo '6' (siempre sin comillas), para especificar FAT16. Por último, en el prompt tipeo 'a', para agregarle la marca de booteo. Reviso que me quede como se puede ver más arriba y con 'w' grabo los cambios en el pen drive.

3.3. Formatear el pen drive. Ahora queda darle formato al pen drive, para poder comenzar a grabarle archivos:

```
# mkfs.vfat -F 16 /dev/sdb1
```

(En este caso utilicé /dev/sdb1 porque lo que formateo es la partición nro. 1, es la única que definí con el fdisk).

3.4. Monto el pen drive (primero creo un directorio en donde montarlo por las dudas):

```
# mkdir -p /mnt/usb
# mount /dev/sdb1 /mnt/usb
```

3.5. Ahora sólo tengo que copiar los archivos en forma recursiva (con sus subdirectorios) de mi directorio 'image'. Por ejemplo, si estoy posicionado en 'work':

```
# cp -r image/* /mnt/usb
```

3.6. Desmonto el pen drive:

```
# umount /mnt/usb
```

3.7. Aquí viene lo más importante: hacer el pen drive booteable. Para eso ejecutamos el programa 'syslinux':

```
# syslinux /dev/sdb1
```

Lo que hace el syslinux es leer el syslinux.cfg y generar un archivo en el directorio raíz del pen drive llamado ldlinux.sys. Al configurar la BIOS para que arranque del pen drive, el syslinux se ejecuta y sigue las directivas definidas en el syslinux.cfg que generamos. Con la versión de syslinux incluída en Ubuntu Feisty (3.11) no es posible leer el archivo de configuración de un subdirectorio; a partir de la versión 3.35 ya está implementado (escanea unos subdirectorios predefinidos buscando el .cfg).

4\. Consideraciones Finales4.1. Si bien no haría falta eliminar completamente el MBR del pen drive (dd if=/dev/zero ...), encontré muchas incompatibilidades con pen drives cuya geometría sea mayor a 1024 cilindros (cabe aclarar que la gran mayoría de los pen drives actuales vienen con el tipo de partición 'e W95 FAT16 (LBA)', y alrededor de 3900 cilindros los de 1GB). La máquina en la que iba a correr el sistema directamente no bootea si el pen drive no tiene menos de 1030 cilindros (encontré pen drives que con 1028 booteaban igual), además de estar formateado en FAT 16. Quizás tenga que ver conque el modo de booteo es "USB-FDD" (siendo FDD: Floppy Disk Drive, y por esto sea el límite de cilindros tan bajo).

La opción de utilizar FAT 16 (tamaño máximo de partición: 2GB) también apunta a tener más compatibilidad con los BIOSes. También encontré incompatibilidades al bootear con otro tipo de máquina mucho más moderna, cuando el pen drive tenía una geometría de 1015 o 1032 cilindros. Para el proyecto en el que hice esto, terminé haciendo imágenes con "dd if=/dev/sdb of=imagen.img" de los pen drives que funcionaban y guardándolos en el HD para luego volver a clonarlos con dd if=imagen.img of=/dev/sdb. Esto particularmente anduvo muy bien.

4.2. Para modificar el filesystem generado, sólo hay que entrar al chroot, y modificarlo a gusto. Por ejemplo, podemos instalar nuevos paquetes que van a estar disponibles en nuestro live-usb (con apt-get), modificar archivos de arranque (/etc/rc.local), es decir, personalizar nuestra distro! :-D Luego, salimos del chroot (comando 'exit'), borramos el archivo image/casper/filesystem.squashfs anterior, y volvemos a generar el filesystem comprimido (ver arriba: "mksquashfs...").

Links Relacionados: [http://es.wikipedia.org/wiki/Chroot](http://es.wikipedia.org/wiki/Chroot) [http://www.pendrivelinux.com/2007/05/31/create-your-own-live-linux-cd-or-usb-distribution/](http://www.pendrivelinux.com/2007/05/31/create-your-own-live-linux-cd-or-usb-distribution/) [https://help.ubuntu.com/community/LiveCDPersistence](https://help.ubuntu.com/community/LiveCDPersistence) [https://help.ubuntu.com/community/LiveCDCustomizationFromScratch](https://help.ubuntu.com/community/LiveCDCustomizationFromScratch) [http://el-directorio.org/DebianLive#head-9df2b36e5793bbfb9353ffbf472fb3e38608935e](http://el-directorio.org/DebianLive#head-9df2b36e5793bbfb9353ffbf472fb3e38608935e) [http://wiki.debian.org/DebianLive/Howto/USB](http://wiki.debian.org/DebianLive/Howto/USB)

Espero que les sirva, y cualquier duda/insulto/comentario, pueden consultarme por mail (o comentando este post).

Saludos!
Marcelo
