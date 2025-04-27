---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2010-08-14T07:14:26+00:00"
guid: https://blog.marcelofernandez.info/?p=936
title: Achicando imágenes de Máquinas Virtuales (KVM-QCow2)
url: /2010/08/achicando-imagenes-de-maquinas-virtuales-kvm-qcow2/

---
## Consolidando Máquinas Físicas a Virtuales

Dentro del mundo de la Virtualización, al momento de consolidar máquinas [\[1\]](#1) lo más sencillo (o lo que primero se le puede ocurrir a uno [\[2\]](#2)) es hacer una imagen bit a bit del disco donde éste se aloja a un archivo del _Host_, mediante alguna herramienta como [dd](http://en.wikipedia.org/wiki/Dd_(Unix)) en Linux:

```
$ dd if=/dev/sdb of=/vms/images/imagen.raw
```

Este mecanismo es generalmente infalible; luego de ésto, uno crea un perfil de una Máquina Virtual (típicamente un archivo XML donde se describen las características del _Guest_), se lo relaciona a esta imagen "cruda" de la ex-Máquina Física, y en un minutos la misma está corriendo como Máquina Virtual sin problemas.

Además, es más común utilizar un formato de imágenes más flexible que el Raw ("crudo"), que soporte características como:

- [Almacenamiento/Crecimiento dinámico](http://en.wikipedia.org/wiki/Virtual_disk_image#Dynamic_Storage_Growth).
- _[Copy-On-Write](http://en.wikipedia.org/wiki/Qcow2)_ (en este caso se trata de la posibilidad de tener una imagen "base" en modo sólo lectura y guardar las diferencias en otra imagen).
- Gestionar _[snapshots](http://en.wikipedia.org/wiki/Snapshot_(computer_storage))_ del disco de cualquier momento dado.
- Compresión al vuelo.
- Encriptación.
- etc.

Es aquí donde cada solución de virtualización [tiene su propio formato](http://en.wikipedia.org/wiki/Virtual_disk_image) para obtener todas o algunas de estas ventajas, según el caso:

- [VirtualBox](http://www.virtualbox.org) tiene al formato [VDI](http://en.wikipedia.org/wiki/VirtualBox#Virtual_Desktop_Image),
- [VMWare](http://www.vmware.com) tiene el [VMDK](http://en.wikipedia.org/wiki/VMDK),
- [KVM+QEmu](http://www.linux-kvm.com/) tiene el [QCow2](http://people.gnome.org/~markmc/qcow-image-format.html),
- etc.

El manejo del almacenamiento en forma dinámica de éstos nos permite que al momento de crear una imagen de un disco de por ejemplo, 80 GB, la imagen ocupe físicamente unos pocos cientos de bytes; a medida que se van guardando archivos en el disco de la VM, éstos son almacenados finalmente en el archivo de imagen, hasta llegar al tope estipulado al momento de crear la imagen de disco (los 80 GB, por ejemplo).

Pero volvamos al caso de la generación de una imagen de un disco físico de un equipo, ¿qué pasa si éste era de 160 GB y sólo tenía 20 GB de información guardada en uso efectivo? El archivo .raw es una copia bit a bit del disco de punta a punta, por lo tanto, su imagen Raw ("cruda") en el Host ocupará 160 GB. Es decir, **este mecanismo no discrimina el espacio ocupado por los archivos de datos del no utilizado y/o de archivos eliminados**.

Bueno entonces, ¿para qué tenemos los formatos de imágenes nativos del _hypervisor_(KVM+QEmu en nuestro caso) que mencionamos anteriormente? ¿Es posible convertir una imagen Raw en una QCow2, para que su imagen ocupe los 20 GB de datos en vez de 160 GB? Sí es posible la conversión; para todas las operaciones con imágenes de disco KVM+QEmu tiene la herramienta [qemu-img](http://wiki.qemu.org/download/qemu-doc.html#disk_005fimages):

```
$ qemu-img convert -f raw imagen.raw -O qcow2 imagen.qcow2
```

Con ese comando convertimos el archivo Raw al formato QCow2, buscando que la imagen a utilizar sea más pequeña , pero en su lugar la mejora de espacio es mucho menor a la esperada (aún si se llegara a dar). Es aquí donde nos encontraremos con un problema: **¡qemu-img no sabe distinguir entre los archivos de datos y el espacio no utilizado/eliminado!** Si la máquina física estuvo en uso por algún tiempo, es probable que no lleguemos ni cerca al archivo "ideal" de 20 GB. Si momentáneamente tuve en ese disco de esa máquina física 60 GB extras de música y fotos familiares, seguramente el archivo QCow2 convertido desde Raw no va a bajar de los 80GB, por ejemplo.

¿Y esto porqué sucede? [La teoría](http://en.wikipedia.org/wiki/Data_remanence) dice que:

- Todo aquel espacio de disco, que viene de fábrica digamos, está compuesto de bytes en cero (0x00).
- Todo aquel espacio de disco que es utilizado primero y luego borrado del disco, no es eliminado físicamente, es decir, no vuelve a cero, sino que queda con el contenido previo. El sistema de archivos en la enorme mayoría de los casos, para ganar velocidad (y mucha) al hacerlo, sólo quita la referencia en el índice del Sistema de Archivos al conjunto de clusters del disco que tiene el contenido real del archivo. Es por eso que uno "no ve" al archivo recientemente eliminado porque el índice de archivos existentes no lo tiene más, pero físicamente el archivo sigue estando.
- QEmu-img reconoce como espacio vacío los bytes en cero.

Sin embargo, tenemos opciones para hacer lo que queremos y no quedarnos sin espacio en el Host rápidamente, nada más que en KVM+QEmu [hay que hacerlo](http://qemu-forum.ipi.fi/viewtopic.php?t=2739) [un poco](http://itpro.haqida.uz/d/admin-notes/20-how-to-shrink-qcow-file.html) [más a mano](http://kerneltrap.org/mailarchive/linux-kvm/2009/3/22/5215854/thread); este proceso se llama en inglés " _Image Shrinking_" ("achicar", "reducir", "contraer" la imagen). [\[3\]](#3) [\[4\]](#4)

## Achicando ("Shrinkeando") imágenes de discos virtuales

Según "la teoría" y los [links](http://qemu-forum.ipi.fi/viewtopic.php?t=2739) [que se han citado](http://itpro.haqida.uz/d/admin-notes/20-how-to-shrink-qcow-file.html), **para poder recuperar espacio ocupado del disco hay que ponerlo en cero**; ¿Y cómo se hace? Fácil, **creando un archivo tan grande como el espacio remanente del disco lleno de ceros**.

### Armando el Caso de Prueba

Vamos a tratar de reproducir el problema y solucionarlo en forma representativa con una imagen Raw de 10 MB, como para que cualquiera pueda seguir estos pasos y entender de qué se trata todo esto.

1) Creamos una imagen Raw de prueba con 10 MB de bytes aleatorios:

```
marcelo@marcelo-laptop:~$ dd if=/dev/urandom of=test.raw bs=1024 count=10000
10000+0 registros de entrada
10000+0 registros de salida
10240000 bytes (10 MB) copiados, 2,24907 s, 4,6 MB/s
marcelo@marcelo-laptop:~$
```

2) Ya que todo disco tiene una tabla de particiones, se la creamos con parted:

```
marcelo@marcelo-laptop:~$ parted test.raw print
AVISO: Usted no es el superusuario. Compruebe los permisos.
Error: /home/marcelo/test.raw: etiqueta de disco no reconocida
marcelo@marcelo-laptop:~$ parted test.raw mktable msdos
AVISO: Usted no es el superusuario. Compruebe los permisos.
marcelo@marcelo-laptop:~$ parted test.raw print
AVISO: Usted no es el superusuario. Compruebe los permisos.
Modelo:  (file)
Disco /home/marcelo/test.raw: 10,2MB
Tamaño de sector (lógico/físico): 512B/512B
Tabla de particiones. msdos

Numero  Inicio  Fin  Tamaño  Tipo  Sistema de ficheros  Banderas

marcelo@marcelo-laptop:~$
```

El "parted test.raw print" muestra la tabla de particiones, y el "parted test.raw mktable msdos" crea una tabla de particiones de tipo MSDOS (hay otros tipos, como por ejemplo [GUID](http://en.wikipedia.org/wiki/GUID_Partition_Table)). Primero vemos que el print da un error, luego creamos la tabla y por último vemos la tabla de particiones sin particiones definidas.

3) Ahora hay que crear una partición en esa tabla, nuevamente con parted:

```
marcelo@marcelo-laptop:~$ parted test.raw mkpart primary 0 10
AVISO: Usted no es el superusuario. Compruebe los permisos.
Aviso: La partición resultante no está debidamente alineada para el mejor rendimiento.
Descartar/Ignore/Cancelar/Cancel? Ignore
marcelo@marcelo-laptop:~$ parted test.raw print
AVISO: Usted no es el superusuario. Compruebe los permisos.
Modelo:  (file)
Disco /home/marcelo/test.raw: 10,2MB
Tamaño de sector (lógico/físico): 512B/512B
Tabla de particiones. msdos

Numero  Inicio  Fin     Tamaño  Tipo     Sistema de ficheros  Banderas
 1      512B    10,2MB  10,2MB  primary

marcelo@marcelo-laptop:~$
```

4) Ahora le damos formato a la partición en la imagen, por ejemplo, NTFS. El parámetro "--fast" es para que no llene de ceros la partición al momento de crearla (sólo para que siga con datos aleatorios) y el "--force" es para que se haga la operación a pesar de que el archivo no es un disco "de verdad".

```
marcelo@marcelo-laptop:~$ mkfs.ntfs --fast --force test.raw
test.raw is not a block device.
mkntfs forced anyway.
The sector size was not specified for test.raw and it could not be obtained automatically.  It has been set to 512 bytes.
The partition start sector was not specified for test.raw and it could not be obtained automatically.  It has been set to 0.
The number of sectors per track was not specified for test.raw and it could not be obtained automatically.  It has been set to 0.
The number of heads was not specified for test.raw and it could not be obtained automatically.  It has been set to 0.
Cluster size has been automatically set to 4096 bytes.
To boot from a device, Windows needs the 'partition start sector', the 'sectors per track' and the 'number of heads' to be set.
Windows will not be able to boot from this device.
Creating NTFS volume structures.
mkntfs completed successfully. Have a nice day.
marcelo@marcelo-laptop:~$ parted test.raw print
AVISO: Usted no es el superusuario. Compruebe los permisos.
Modelo:  (file)
Disco /home/marcelo/test.raw: 10,2MB
Tamaño de sector (lógico/físico): 512B/512B
Tabla de particiones. loop

Numero  Inicio  Fin     Tamaño  Sistema de ficheros  Banderas
 1      0,00B   10,2MB  10,2MB  ntfs

marcelo@marcelo-laptop:~$
```

6) Perfecto, ya tenemos nuestro "mini disco" NTFS para jugar. Ahora vamos a montarlo y ver qué tiene:

```
marcelo@marcelo-laptop:~$ sudo mount -o loop test.raw /mnt
[sudo] password for marcelo:
marcelo@marcelo-laptop:~$ mount
/dev/sda1 on / type ext4 (rw,errors=remount-ro)
[...]
/dev/loop0 on /mnt type fuseblk (rw,nosuid,nodev,allow_other,blksize=4096)
marcelo@marcelo-laptop:~$ tail /var/log/syslog
[...]
Aug 14 02:28:57 marcelo-laptop ntfs-3g[19770]: Version 2010.3.6 external FUSE 28
Aug 14 02:28:57 marcelo-laptop ntfs-3g[19770]: Mounted /dev/loop0 (Read-Write, label "", NTFS 3.1)
Aug 14 02:28:57 marcelo-laptop ntfs-3g[19770]: Cmdline options: rw
Aug 14 02:28:57 marcelo-laptop ntfs-3g[19770]: Mount options: rw,silent,allow_other,nonempty,relatime,fsname=/dev/loop0,blkdev,blksize=4096
Aug 14 02:28:57 marcelo-laptop ntfs-3g[19770]: Ownership and permissions disabled, configuration type 1
marcelo@marcelo-laptop:~$  ls -l /mnt
total 0
marcelo@marcelo-laptop:~$
```

7) Ahora estamos como si hubiéramos hecho el dd de un disco físico en nuestro Host con KVM+QEmu. Como se ha visto, la imagen no tiene nada en el sistema de archivos pero físicamente está lleno de bytes aleatorios y ocupa 10 MB, el tamaño completo. ¿Qué pasa si lo convertimos a QCow2? ¿Se cumple "la teoría"?

```
marcelo@marcelo-laptop:~$ sudo umount /mnt
marcelo@marcelo-laptop:~$ qemu-img convert -f raw test.raw -O qcow2 test.qcow2
marcelo@marcelo-laptop:~$ ls -l
[...]
-rw-r--r--  1 marcelo marcelo 10420224 2010-08-14 02:37 test.qcow2
-rw-r--r--  1 marcelo marcelo 10240000 2010-08-14 02:28 test.raw
[...]
marcelo@marcelo-laptop:~$ qemu-img info test.raw
image: test.raw
file format: raw
virtual size: 9.8M (10240000 bytes)
disk size: 9.8M
marcelo@marcelo-laptop:~$ qemu-img info test.qcow2
image: test.qcow2
file format: qcow2
virtual size: 9.8M (10240000 bytes)
disk size: 9.8M
cluster_size: 65536
marcelo@marcelo-laptop:~$
```

Sí, se cumple... es más, la imagen QCow2 ocupa más espacio que la Raw (por ahora). Hemos podido reproducir el problema.

### Llenando de Ceros el espacio vacío - Wiper.py

Bien, ahora se me ocurrió armar un script en Python para que me ayude a automatizar el proceso de crear un archivo con ceros hasta que me quede sin espacio para luego borrarlo. Lo llamé "wiper.py". El código fuente es el siguiente:

```
#!/usr/bin/env python
# coding: utf-8

# Wiper
# Copyleft 2010 - Licencia BSD
# Autor: Marcelo Fernández.
# Email: marcelo.fidel.fernandez@gmail.com

# This script creates a file which fills the available disk with zero bytes,
# and when the disk is full, the file is deleted.
# This allow to convert a raw partition to qcow2 'shrinking' the real image

import sys, os, errno

FILENAME = 'wiper.000'
STEP_SIZE = 4096

if __name__ == '__main__':
    if len(sys.argv) != 2:
        print 'usage: python wiper.py path_mounted_image'
        sys.exit(1)
    fat_file_path = os.path.join(sys.argv[1], FILENAME)
    try:
        fat_file = open(fat_file_path, 'wb', STEP_SIZE)
    except Exception, e:
        print 'There was an error opening the file in %s: %s' % (fat_file_path, str(e))
        sys.exit(2)
    print 'Temp file %s open for writing. Filling disk...' % fat_file_path
    try:
        while True:
            fat_file.write('\x00' * STEP_SIZE)
    except EnvironmentError, env_error:
        if env_error.errno == errno.ENOSPC:
            print 'No space left on device, good! Deleting temp file...'
        else:
            # There was another error different from 'No space available',
            # anyway we'll alway try to delete the fat_file
            print 'Abnormal error: ', oserror
    except Exception, e:
        # Rarely, but this could happend too
        print 'Exception: ' + repr(e)
    fat_file.close()
    try:
        os.unlink(fat_file_path)
    except Exception, e:
        print 'There was a problem deleting the temp file.'
    else:
        print 'Temp file erased ok.'
```

Básicamente hace lo necesario, el STEP\_SIZE es importante para que la operación sea bastante más rápida que escribiendo de a un byte. Vamos a ejecutar el script y convertir nuevamente el archivo Raw a QCow2 con qemu-img a ver qué sucede:

```
marcelo@marcelo-laptop:~$ sudo mount -o loop test.raw /mnt
marcelo@marcelo-laptop:~$ python wiper.py /mnt/
Temp file /mnt/wiper.000 open for writing. Filling disk...
No space left on device, good! Deleting temp file...
Temp file erased ok.
marcelo@marcelo-laptop:~$ ls -l /mnt
total 0
marcelo@marcelo-laptop:~$ sudo umount /mnt
marcelo@marcelo-laptop:~$ qemu-img convert -f raw test.raw -O qcow2 test_shrinked.qcow2
marcelo@marcelo-laptop:~$ ls -l
[...]
-rw-r--r--  1 marcelo marcelo 10420224 2010-08-14 02:42 test.qcow2
-rw-r--r--  1 marcelo marcelo 10240000 2010-08-14 02:58 test.raw
-rw-r--r--  1 marcelo marcelo  3014656 2010-08-14 02:58 test_shrinked.qcow2
marcelo@marcelo-laptop:~$ sudo mount -o loop test.raw /mnt
marcelo@marcelo-laptop:~$ df -h
S.ficheros            Tamaño Usado  Disp Uso% Montado en
/dev/sda1              16G  6,7G  7,7G  47% /
[...]
/home/marcelo/test.raw
                      9,8M  2,5M  7,3M  26% /mnt
marcelo@marcelo-laptop:~$
```

Bueno, ahora sí, el archivo QCow2 ocupa un cuarto del archivo Raw (2,5 MB). ¿En qué se gastan estos 2.5MB? Seguramente en los índices del sistema de archivos para poder gestionar el espacio libre. Es altamente probable que más que este espacio recuperado no podamos ganar, ya que si tocamos estos índices, corrompemos el sistema de archivos; recordemos que en test.raw puede haber un Sistema Operativo completo con todos sus archivos de datos.

De todas maneras, esto puede variar según el sistema de archivos de la imagen (NTFS en este caso) y los parámetros con los cuales se está manejando el mismo (el tamaño de bloque y de cluster incide mucho en el espacio que ocupan los índices de los Sistemas de Archivos en general).

## Conclusiones

Es bueno saber que también se puede usar el comando dd para crear un archivo lleno de ceros en la partición ntfs de pruebas montada, con el comando "dd if=/dev/zero of=/mnt/borrame.000" y después borrar el archivo en cuestión, pero puede hacerse interminable el "llenado" si no pasamos como parámetro un block size relativamente grande, como de 4096 bytes:

```
marcelo@marcelo-laptop:~$ time dd if=/dev/zero of=/mnt/borrame.000 bs=4096
dd: escribiendo «/mnt/borrame.000»: No hay espacio libre en el dispositivo
1863+0 registros de entrada
1862+0 registros de salida
7626752 bytes (7,6 MB) copiados, 0,144428 s, 52,8 MB/s

real	0m0.149s
user	0m0.000s
sys	0m0.030s
marcelo@marcelo-laptop:~$
```

Prueben a pasarle bs=1, a ver cuánto tarda.... (lo pueden cortar con Ctrl+C). :-)

Una cuestión más que queda dando vueltas es que esta situación de espacio desperdiciado por la imagen se da también luego de un tiempo de usar la Máquina Virtual, si se da el caso de que se libere espacio en forma masiva; como caso, el ejemplo de un disco de 80 GB con 20 GB dedicados al Sistema Operativo más 60 GB de fotos y música eliminados recientemente.  Aquí también será útil correr wiper.py en la imagen montada y volver a ejecutar "qemu-img convert" para _shrinkear_ la imagen.

En fin, espero que este post le sea de utilidad a aquellos que trabajamos con tan formidable _hypervisor_ como lo es KVM y su gestor de I/O como lo es QEmu.

Saludos

\[1\] Si bien a un Linux se lo puede consolidar más fácilmente, sólo copiando todos los archivos con rsync, o "cp -a", con Windows este método no funciona, y hay que copiar el disco o al menos la partición completa. La idea al usar dd es abstraerse del SO.
\[2\] En vez de dd se puede usar una herramienta de clonación de discos/particiones que "entienda" o "interprete" el filesystem, como [Partimage](http://www.partimage.org/Main_Page) para intentar generar directamente una imagen Raw con el espacio no utilizado en cero; pero nunca lo probé, ni me parece un método muy "limpio", ya que estamos insertando un software algo complejo en el medio de algo que de cualquier otra manera es conceptualmente simple y repetible. En el único caso que lo probaría es si estoy falto de espacio de disco en el Host. Si alguien lo prueba, me avisa. :-)
\[3\] Creo que "comprimir" no es la palabra apropiada porque da la idea al lector de que hay un proceso (que implementa un algoritmo) de [compresión de datos](http://en.wikipedia.org/wiki/Data_compression) sin pérdida, como por ejemplo, [LZ](http://en.wikipedia.org/wiki/Lempel-Ziv). Sin embargo, aquí lo que se necesita es recuperar el espacio no utilizado de la imagen, para obtener un tamaño de imagen menor.
\[4\] Como nota al margen, VMWare tiene en sus VMWare Tools una opción para hacer "Shrink" de la imagen, pero ¿cómo se divierte un Sysadmin con ganas de aprender si sólo tiene que hacer click en un botón que diga "Shrink Image"? :-P
