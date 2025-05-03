---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2011-01-15T00:26:20+00:00"
guid: https://blog.marcelofernandez.info/?p=1059
title: Virtualizando sólo una partición de una máquina física
url: /2011/01/virtualizando-solo-una-particion-de-una-maquina-fisica/

---
Hace poquito un amigo me contó que estuvo leyendo [el post anterior](/2010/08/achicando-imagenes-de-maquinas-virtuales-kvm-qcow2/) de Consolidación y "Shrinking" de discos físicos para pasarlos a una máquina virtual, pero tenía un pequeño obstáculo relacionado con el espacio en disco necesario, que más o menos era así:

> Quería consolidar un disco real de 80 GB que tiene XP instalado en su C:, pero en total el disco tiene 4 particiones (primarias). Sólo me interesaba el disco C:, con lo que ahorraba espacio en el proceso ya que la partición del C: sólo ocupa 25 GB.
>
> Entonces hice la copia con "dd if=/dev/sdb1 of=imagen.raw". Como uso VirtualBox, con un comando lo convertí de .raw a .vdi. Pero no logré bootear.
>
> Hice varias pruebas, siempre con VirtualBox y no lo logré. Supongo que es porque no se copió el MBR. En cambio, cuando copié el disco completo, booteó sin problemas.
>
> La pregunta es: ¿Cómo debería haber hecho para trabajar solo con esa partición y de esa forma evitar trabajar con los 80GB?

Bueno, el problema era que, efectivamente, al hacer el dd sobre /dev/sdb1 sólo se copian los bytes dentro de la partición y no el [MBR](http://en.wikipedia.org/wiki/Master_boot_record) +Bootloader (la vieja " [pista 0](http://en.wikipedia.org/wiki/Track_0)" del disco). **Para resolver el problema habría que copiar los bytes de la pista 0 más los bytes de la partición en cuestión**, siguiendo estos pasos:

Ejecutar el comando parted sobre /dev/sdb (en mi caso, voy a usar /dev/sda como ejemplo):

```
marcelo@jupiter:~$ sudo parted /dev/sda u s print
Modelo: ATA WDC WD3200BEVT-0 (scsi)
Disco /dev/sda: 625142448s
Tamaño de sector (lógico/físico): 512B/512B
Tabla de particiones. msdos

Numero  Inicio     Fin         Tamaño      Tipo      Sistema de ficheros  Banderas
 1      63s        32001479s   32001417s   primary   ext4                 arranque
 2      32001541s  94494329s   62492789s   extended
 5      32001543s  94494329s   62492787s   logical   ext4
 3      94494330s  98494514s   4000185s    primary   linux-swap(v1)
 4      98494515s  625137344s  526642830s  primary   ext4
```

El parámetro "print" le dice a parted que me imprima la tabla de particiones, y "u s" que imprima los tamaños en sectores (que en los discos <=1.0TB son siempre de 512 bytes, sino mismo parted lo dice en la salida, ver [este post](/2010/06/discos-rigidos-con-sectores-de-4kb-en-linux/) para más detalles).

De esta manera se tienen todos los sectores a copiar de /dev/sda directamente; en mi caso, 63 sectores de la pista 0, que contiene al MBR, más los de la primera partición, 32001417, total: 32001480 sectores.

Luego el dd que sólo copia los bytes que nos interesan (Pista 0 + Partición "C:") es fácil:

```
sudo dd if=/dev/sda of=imagen.raw bs=512 count=32001480
```

Y listo. Eso sí, luego hay que particionar la imagen con Parted, Fdisk, [GParted](http://gparted.sourceforge.net/) o el software que se quiera utilizar a tal efecto. Es necesario hacerlo porque el MBR copiado va a reportar las 4 particiones que se tenían previamente en el disco y ahora sólo se va a disponer de una sola.

Asumo que este procedimiento también se puede aplicar para copiar una partición que no sea la primera, usando las opciones "skip" y "append" del comando dd; por ejemplo, si quisiera hacer un disco de sólo mi partición 5, tendría que:

1. Copiar la pista 0 en un archivo con algo así como "dd if=/dev/sda of=imagen.raw bs=512 count=63".
1. Ejecutar otro dd con la partición a copiar, salteando ("skipeando") N sectores con la opción "skip" y especificando la opción "append" para agregar al archivo anterior de salida: "dd if=/dev/sda of=imagen.raw bs=512 count=62492787 skip=32001543 append".

Lógicamente esto último no lo probé, acepto comentarios de si funcionó o no ;-)

Espero que a alguien le sirva...

¡Saludos!
