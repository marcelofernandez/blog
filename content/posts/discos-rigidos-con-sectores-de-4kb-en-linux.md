---
author: mfernandez
category:
  - codear
  - linux
  - tests
  - ubuntu-ar
date: "2010-06-14T02:52:03+00:00"
guid: https://blog.marcelofernandez.info/?p=710
title: Discos Rígidos con Sectores de 4KB en Linux
url: /2010/06/discos-rigidos-con-sectores-de-4kb-en-linux/

---
**Actualización (Julio 2010)**: Armé y redacté no tan informalmente este post en forma de artículo; el mismo está disponible para su consulta, crítica y mejoras en la sección de [Publicaciones](/publicaciones/) del sitio.

## Los nuevos discos de Western Digital [![](/wp-content/uploads/2010/06/wdfDesktop_CaviarGreen_SATA64.jpg)](http://www.wdc.com/en/products/products.asp?driveid=772)

En estos días tuve la oportunidad de comprar y configurar una máquina Ubuntu con un disco rígido Western Digital de 1,5 Terabytes (1500 Gigabytes aprox.); más precisamente el modelo [WD15EARS](http://www.wdc.com/en/products/products.asp?driveid=772), con 64 MB de caché, velocidad de rotación que varía entre 5400 y 7200 RPMs (llamado " [IntelliSeek](http://www.wdc.com/en/flash/index.asp?family=intelliseek)"), interfaz SATA II (3 Gb/seg), y que incluye una nueva tecnología de la marca conocida como " [Advanced Format Drive](http://www.wdc.com/en/products/advancedformat/)" (paper [aquí](http://www.wdc.com/wdproducts/library/?id=216&type=87)).

Si bien yo pensé que estaba comprando un disco más aunque sólo de mayor capacidad, luego al mirarlo [más de cerca](http://techreport.com/articles.x/15769) en su etiqueta decía:

[![](/wp-content/uploads/2010/06/WD-Align_chart_r2.jpg)](http://www.wdc.com/en/products/advancedformat/)

Resumiendo, dice que:

- Si va a utilizar el disco con Windows XP con una sola partición en el disco, sólo ponga el Jumper en los pins 7-8 del HD.
- Si va a utilizar el disco con Windows XP con más de una partición en el disco, use la utilidad "WD Align".
- **Si va a utilizar otro SO (Windows Vista/7, Mac OS X, o Linux), el disco no requiere preparación adicional para el particionamiento.**

"Bueno, total no uso Windows", pensé. Particioné y formatié con el [GParted](http://gparted.sourceforge.net/) el disco y me puse a utilizarlo normalmente. No noté nada raro, hasta que unos días después googleando sobre experiencias con éste disco y Linux me encontré con [este hilo en el foro de WD](http://community.wdc.com/t5/Desktop/Problem-with-WD-Advanced-Format-drive-in-LINUX-WD15EARS/td-p/6395/) (" _Problem with WD Advanced Format drive in LINUX (WD15EARS)_"). Al parecer, mucha gente utilizando Linux con estos discos notaba una lentitud excesiva en cada cosa que hacía, como si hubiera un problema, no del hardware fallando y perdiendo datos, sino de demoras muy grandes en lectura/escritura. Resultó que **aún usando Linux, el disco sí requiere preparación adicional, al contrario de lo que dice el fabricante**, como veremos más adelante.

## Sectores de 4 KBytes en vez de 512 Bytes, ¿Por qué?

 [Desde los primeros días](http://en.wikipedia.org/wiki/Cylinder-head-sector#cite_note-0) de los discos rígidos, la mínima unidad de almacenamiento direccionable fue el [sector](http://en.wikipedia.org/wiki/Cylinder-head-sector), de **512 Bytes** de capacidad. Esta granularidad se eligió porque siempre fue un buen balance entre la [fragmentación interna](http://elezeta.net/2004/09/16/fragmentacin-de-que-se-trata/) de los archivos [\*](#nota1) y el manejo físico del disco relativo a la corrección de errores, flags de inicio/fin del sector más la separación inter-sector ( _gap_). Toda la industria de la PC y el software creado para ella se apoyó en este estándar de facto, y casi ningún utilitario, BIOSes, ni Sistema Operativo se pensaron para un posible cambio... sólo hasta hace unos pocos años.

Western Digital es una de las primeras marcas en sacar al mercado discos con sectores 8 veces más grandes que los anteriores, de 4 KBytes (4096 Bytes); estos discos son etiquetados como que poseen "Advanced Format Technology" ("Tecnología de Formato Avanzado"), y **es lo primero que uno debería empezar a mirar al comprar discos nuevos de gran tamaño (1 TB o superior), ya sean de WD o de otros fabricantes**. Casualmente (o no tanto), esto incluye al disco que compré.

El motivo del cambio, según lo que explica [este excelente post de AnandTech](http://www.anandtech.com/show/2888), se debe a que existen **3 factores esenciales que deben compensarse para buscar capacidades de almacenamiento cada vez mayores** en el mismo tamaño de disco (que por lo general es de 3,5 pulgadas):

1. [Densidad de área](http://en.wikipedia.org/wiki/Memory_storage_density): Cuántos bits se pueden guardar en un área determinada (Bits/Pulgada cuadrada). Hasta ahora, con la [Grabación Perpendicular](http://en.wikipedia.org/wiki/Perpendicular_recording), estamos en el orden de unos 300/500 Gigabits por pulgada cuadrada ( [1](http://arstechnica.com/hardware/news/2006/09/7765.ars), [2](http://arstechnica.com/science/news/2010/05/new-hard-drive-write-method-packs-in-one-terabyte-per-inch.ars)).
1. [Relación Señal/Ruido](http://en.wikipedia.org/wiki/Signal-to-noise_ratio): Al leer de los platos del disco, pueden ocurrir fallos, ya que el almacenamiento magnético en definitiva es analógico; y la señal, para ser convertida desde/hacia binario, debe ser procesada. Cuanto mejor sea la relación de la señal con respecto al ruido en el momento de leer o escribir en los platos, más confiable es la operación.
1. El uso del [Código de Corrección de Errores - ECC](http://en.wikipedia.org/wiki/Forward_error_correction): Cada sector del disco incluye un área reservada para almacenar el ECC, imprescindible para recuperarse ante cualquier error de lectura/escritura.

A medida que la Densidad del Area se incrementa, los sectores (siempre de 512 Bytes) lógicamente se reducen en el área que ocupan físicamente. Esto hace que se incremente el Ruido con respecto a la Señal porque las señales son más débiles y hay más interferencia de los datos adyacentes; por lo tanto el valor de SNR disminuye, y a su vez, la probabilidad de errores de lectura aumenta. Entonces, es necesario mejorar la capacidad del ECC para detectar y corregir errores, generalmente agregándole más bits. Esto requiere de más espacio físico reservado para un sector (siempre de 512 Bytes), y aquí volvemos a empezar.

Lo que sucede es que aquí está el punto; se está llegando a un límite donde no se puede seguir con sectores de 512 Bytes y aumentar el tamaño total del disco, ya que todo este nuevo espacio obtenido con una mayor densidad termina no siendo utilizable, sino que será mayormente para el ECC (es decir, redundancia para contemplar posibles errores).

La solución al problema es que, para almacenar más información globalmente, hay que incrementar la eficiencia del ECC. Y esto se logra haciendo que éste abarque más datos que sólo 512 Bytes; **el ECC es mucho más eficiente (ocupa menos espacio en comparación) si su código de corrección abarca más datos**, digamos, 4096 Bytes.

Por ejemplo, para detectar y corregir corregir 4096 Bytes divididos en 8 sectores de 512 Bytes (de la vieja forma), necesitamos "gastar" 320 Bytes de ECC (ya que tenemos 40 Bytes por cada ECC de 512 Bytes), mientras que si usamos 1 sector de 4096 Bytes sólo vamos a usar 100 Bytes de ECC. Como se puede ver, uno se ahorra 220 Bytes de _overhead_ por cada 4KB que tiene el disco para guardar cosas; en 1500 GB (= 1.500.000.000 KB / 4 = 375.000.000 sectores de 4 KB \* 0,22 KB) son 82,5 GB más de espacio disponible para almacenar datos de usuario y no ECC (un 5,5% más). Esto y sin contar el espacio "desperdiciado" para los _gaps_ entre sectores y el flag de sincronización/inicio de sector (para 4 KB, antes eran 8 y ahora es sólo 1). Además, estos 100 Bytes de ECC mejoran en un 50% la capacidad de detectar errores en "ráfaga" comparado con el anterior, es decir, **el nuevo es un mejor y más eficiente ECC**.

Por todo esto, para tamaños tan grandes de disco, usar sectores de 4KB nos permite aprovechar de manera más eficiente la mayor densidad del área que disponemos. **¿Y por qué 4 KB? No es un número al azar**; coincide con el tamaño de las páginas de memoria en la arquitectura x86 y con el tamaño de [cluster](http://en.wikipedia.org/wiki/Data_cluster) por defecto de la mayoría de los sistemas de archivos que pululan por ahí, con lo cual la velocidad de transferencia de páginas desde/hacia el disco no se ve afectada, y la fragmentación interna de los archivos almacenados es la misma que con sectores de 512 Bytes.

Quizás con el gráfico se entienda un poco más; allí se ve cómo ocupan más lugar los 8 sectores de 512 Bytes puestos a la par del sector de 4 KB:

[![](/wp-content/uploads/2010/06/4kview.png)](http://www.anandtech.com/show/2888)

Nuevamente (espero comentarios y corrijo en todo caso), toda esta info está bien explicada en el [artículo de AnandTech](http://www.anandtech.com/show/2888), en [este artículo de Ars Technica](http://arstechnica.com/microsoft/news/2010/03/why-new-hard-disks-might-not-be-much-fun-for-xp-users.ars), en el [Paper de WD](http://www.wdc.com/wdproducts/library/?id=216&type=87) y en [este artículo de IBM](http://www.ibm.com/developerworks/linux/library/l-4kb-sector-disks/index.html?ca=dgr-lnxw074KB-Disksdth-LX).

Bien, "¿Y qué gano con sectores más grandes? ¿en qué me afecta?", es lo primero que uno se pregunta. Claramente, y por lo explicado recientemente, **se gana en confiabilidad y capacidad de almacenamiento hoy y a futuro**. **¿Cuáles son las contras? básicamente, el proceso de migración**, que como ya se dijo, deben afrontarse desde varias capas de software, desde el BIOS, pasando por el Sistema Operativo y llegando a las herramientas de defragmentación, clonado y administración de discos.

## ¿Y qué problema hay con Linux? Particiones desalineadas.

Teniendo en mente esto, esta serie de discos de WD **emula ser un disco con sectores "lógicos"de 512 bytes, pero en realidad trabaja internamente con sectores "físicos" de 4 KB**. De esta manera, los sectores de 512 bytes lógicos se ven así, dentro de uno de 4 KB:

[![](/wp-content/uploads/2010/06/512B.png)](http://www.anandtech.com/show/2888) Recordemos, la mínima unidad "direccionable" real del disco son 4 KB (un sector), y el tamaño de cluster por defecto (la mínima unidad "direccionable" por el sistema de archivos) son 4 KB; esto quiere decir en definitiva que el disco, cada vez que lee y escribe lo hace en unidades de 4 KB. Luego le agregamos la capa de emulación, para hacerlo compatible con los SOs que tenemos ahora. Bien, supongamos que vamos a crear una sola partición en el disco. **¿Qué pasa si esta partición, donde vamos a guardar los datos, comienza en el sector 1 (o cualquier sector no múltiplo de 8 ) del disco en vez del sector 0?** Va a pasar esto:

[![](/wp-content/uploads/2010/06/sector.png)](http://www.anandtech.com/show/2888)

Lo que sucede es que el primer cluster empieza y termina en dos sectores físicos; está "desalineado" con respecto a ellos. En realidad, todos los clusters de la partición estarán de esta forma, comenzando y finalizando a "destiempo" respecto de los bloques físicos. El problema que esto conlleva es que si el SO va a escribir algo en el disco (donde el "algo" es como mínimo generalmente un cluster de 4 KB), supongamos el cluster resaltado en azul, esto se transforma físicamente en:

1. Leer los 4 KB del sector físico 0.
1. Modificar los 7 sectores lógicos afectados por esta operación.
1. Grabar los 4 KB del sector físico 0.
1. Leer los 4 KB del sector físico 1.
1. Modificar el 8vo sector lógico.
1. Grabar los 4 KB del sector físico 1.

Esto es, 2 operaciones de Leer-Modificar-Grabar ( [RMW](http://en.wikipedia.org/wiki/Read-modify-write)) atómicas, una para cada sector físico, que involucra una vuelta ( _spin_) de disco por cada lectura/escritura de la lista enumerada. Es decir, **que una partición comience en un sector lógico de 512 Bytes no múltiplo de 8 hace excesivamente lento el acceso al disco porque las operaciones llevan mucho, mucho más tiempo que antes**.

Y la cuestión reside aquí; si bien Linux a esta altura ya está preparado para manejar discos con sectores de 4 KB, **el** **problema es que el disco "le dice" a Linux que tiene sectores físicos de 512 bytes por la emulación,** **[cuando internamente trabaja con 4 KB](http://thread.gmane.org/gmane.linux.utilities.util-linux-ng/2926)**:

```
marcelo@marcelo:~$ sudo hdparm -I /dev/sdb | grep Sector
	Logical/Physical Sector size:           512 bytes
```

¿Y cuál es la consecuencia? La posibilidad de no tener los sectores alineados. Fdisk y cualquier software particionador de discos de Linux comienza la primer partición en el sector 63 de aquellos discos que reconoce como de sectores de 512 Bytes [\*\*](#nota2). Esto hace que el disco funcione muy lento, como se describía en [el foro de WD](http://community.wdc.com/t5/Desktop/Problem-with-WD-Advanced-Format-drive-in-LINUX-WD15EARS/td-p/6395/) que cité al comienzo.

## Cómo particionar estos discos en general y en Linux en particular

Bueno, ¿cómo se hace para crear particiones de manera alineada? Es relativamente fácil. Según [Ted Ts'o](http://thunk.org/tytso/blog/2009/02/20/aligning-filesystems-to-an-ssds-erase-block-size/), donde explica que hay un problema parecido con los nuevos discos [SSD](http://en.wikipedia.org/wiki/Solid_state_storage), hay que ejecutar [fdisk](http://tldp.org/HOWTO/Partition/fdisk_partitioning.html) con los parámetros "-H 224 -S 56 /dev/sdX", siendo /dev/sdX el disco en cuestión; esto hace que todas las particiones se creen en la sesión interactiva de fdisk en sectores múltiplos de 8. Otra opción es usar [GNU Parted](http://www.gnu.org/software/parted/index.shtml) con los parámetros "unit s", y de esta manera deja a uno configurar el primer sector de cada partición (más info [acá](http://www.gnu.org/software/parted/manual/html_node/Running-Parted.html#Running-Parted)).

En mi caso necesitaba crear 4 particiones. Este es un ejemplo de particiones bien alineadas, el último comando es para mostrar el tamaño de cada partición nada más:

```
marcelo@marcelo:~$ sudo parted /dev/sdb unit s print
Modelo: ATA WDC WD15EARS-00S (scsi)
Disco /dev/sdb: 2930277168s
Tamaño de sector (lógico/físico): 512B/512B
Tabla de particiones. msdos

Numero  Inicio       Fin          Tamaño       Tipo     Sistema de ficheros  Banderas
 1      56s          41959679s    41959624s    primary  ext4
 2      41959680s    46161919s    4202240s     primary
 3      46161920s    1673570303s  1627408384s  primary  ext4
 4      1673570304s  2930265855s  1256695552s  primary                       raid

marcelo@marcelo:~$ sudo fdisk -lu /dev/sdb

Disco /dev/sdb: 1500.3 GB, 1500301910016 bytes
224 cabezas, 56 sectores/pista, 233599 cilindros, 2930277168 sectores en total
Unidades = sectores de 1 * 512 = 512 bytes
Tamaño de sector (lógico / físico): 512 bytes / 512 bytes
Tamaño E/S (mínimo/óptimo): 512 bytes / 512 bytes
Identificador de disco: 0x00094da1

Dispositivo Inicio    Comienzo      Fin      Bloques  Id  Sistema
/dev/sdb1   *          56    41959679    20979812   83  Linux
/dev/sdb2        41959680    46161919     2101120   82  Linux swap / Solaris
/dev/sdb3        46161920  1673570303   813704192   83  Linux
/dev/sdb4      1673570304  2930265855   628347776   fd  Linux raid autodetect

marcelo@marcelo:~$ sudo parted /dev/sdb print
Modelo: ATA WDC WD15EARS-00S (scsi)
Disco /dev/sdb: 1500GB
Tamaño de sector (lógico/físico): 512B/512B
Tabla de particiones. msdos

Numero  Inicio  Fin     Tamaño  Tipo     Sistema de ficheros  Banderas
 1      28,7kB  21,5GB  21,5GB  primary  ext4
 2      21,5GB  23,6GB  2152MB  primary
 3      23,6GB  857GB   833GB   primary  ext4
 4      857GB   1500GB  643GB   primary                       raid
```

Nuevamente: Lo más importante para que los clusters estén alineados con los sectores físicos del disco es que cada partición debe comenzar en un sector múltiplo de 8, como el 56, ﻿41959680, 46161920 y 1673570304 de este caso.

## Conclusión

Bueno, fue un artículo muy largo, donde me pareció que servía explayarse en el porqué de los sectores más grandes y qué problemas trae.  Tuve la grata oportunidad de intercambiar algunos mails al respecto con [Aleksander Adamowski](http://olo.org.pl/dr/cv_eng), la persona que en la lista de [util-linux-ng](http://userweb.kernel.org/~kzak/util-linux-ng/) " [descubrió](http://thread.gmane.org/gmane.linux.utilities.util-linux-ng/2926)" el inconveniente de la lentitud con esta serie de discos WD a base de un lote de pruebas disponible [aquí](http://olo.org.pl/files/hw/postmark-automated/).

Aleksander me recomendó, en resumidas cuentas, al trabajar con discos > 1 TB "sospechosos" de tener sectores de 4 KB:

- "Las unidades de medida son críticas. Asegúrate que estás realmente operando a nivel de sectores [\\*\\*\\*](#nota3)
- Después , hacer un test de performance es buena idea.
- Para esto, primero trata de crear una partición desalineada, crea un sistema de archivos, y ejecuta el benchmark de [postmark](http://www.shub-internet.org/brad/FreeBSD/postmark.html) usando el archivo de configuración que publiqué ( [http://olo.org.pl/files/hw/postmark-automated/postmark-quick.conf](http://olo.org.pl/files/hw/postmark-automated/postmark-quick.conf) \- por supuesto, modifica la opción "location" en ese archivo acorde al disco a comprobar.
- Luego, borra todas las particiones y haz lo mismo en una partición alineada. Los resultados deben ser **mucho** mejores en cuanto al rendimiento. Si no lo son, el disco probablemente tiene sectores físicos de 512 Bytes."

En resumen, hay que estar atentos. Sería bueno que los futuros discos que salgan con sectores de 4 KB informaran al SO qué estructura real tienen, y eliminar el "modo compatibilidad" de una vez por todas.

Espero que les sirva.
Saludos

\\* En tiempos donde la capacidad de los discos era muy pequeña comparada con los de ahora, el tamaño de un cluster del sistema de archivos era igual al de un sector.

\\*\\* La utilización del sector 63 corresponde a que generalmente (y por herencia histórica del [modelo de direccionamiento CHS](http://en.wikipedia.org/wiki/Cylinder-head-sector)) es el primer sector del track número 1. El track 0 siempre se utilizó para el [MBR / Master Boot Record](http://en.wikipedia.org/wiki/Master_boot_record).

\\*\\*\\* Esto porque por defecto las herramientas de particionamiento Linux no trabajan con sectores; fdisk trabaja con cilindros por ejemplo.
