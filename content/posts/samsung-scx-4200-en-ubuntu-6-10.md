---
author: mfernandez
category:
  - codear
  - linux
date: "2007-02-27T03:35:00+00:00"
guid: https://blog.marcelofernandez.info/?p=45
title: Samsung SCX-4200 en Ubuntu 6.10
url: /2007/02/samsung-scx-4200-en-ubuntu-610/

---
Hola!

Fue un laargo tiempo sin postear, entre vacaciones, laburo y asuntos personales, el blogueo quedó a un lado. Ahora ya casi que "volví" y espero seguir escribiendo...
[![](http://1.bp.blogspot.com/_nDZ247g0qSM/ReOWgIVHRuI/AAAAAAAAADA/504D5SV2X10/s200/h983.jpg)](http://1.bp.blogspot.com/_nDZ247g0qSM/ReOWgIVHRuI/AAAAAAAAADA/504D5SV2X10/s1600-h/h983.jpg)
En esta oportunidad les comento que compré una Impresora [Multifunción Samsung SCX-4200](http://www.samsung.com/ar/products/printers/multifunction/scx_4200.asp), dado que tiene un costo relativamente bajo, soporta Linux (al menos x86 y x86-64, y tiene el pingüinito en la caja!), y además de imprimir con calidad láser, también fotocopia y escanea. Las [reviews](http://www.vnunet.com/computeractive/hardware/2161916/review-samsung-scx-4200-multi) [que](http://reviews.cnet.com/Samsung_SCX_4200/4505-3181_7-31628074.html) [encontré](http://www.neoseeker.com/resourcelink.html?rlid=137144) en mi opinión eran bastante alentadoras (digamos que sus "desventajas" para mí no eran problema), así que la traje a mi casa.

Como siempre hago antes de comprar algo, leí si alguien había tenido dificultades al instalarla en Linux, [Debian](http://www.debian.org/) y/o [Ubuntu](http://www.ubuntu.com/) específicamente. Encontré [varios](http://ubuntuforums.org/showthread.php?t=245545&highlight=samsung+scx+4200) [links](http://ubuntuforums.org/showthread.php?t=341621&highlight=samsung+scx+4200), [muchos](http://news.u32.net/articles/2007/02/05/the-samsung-unified-linux-driver-installer-on-ubuntu) [que](http://www.ubuntuforums.org/showthread.php?t=287747) [se](http://mattgordon.wordpress.com/howto/installing-a-samsung-printer/) [apuntaban](http://openprinting.org/show_printer.cgi?recnum=Samsung-SCX_4200) [entre](http://jacobo.tarrio.org/Samsung_SCX-4200_on_Debian) [sí](http://www.elijahlofgren.com/ubuntu/#scx-4100), y linuxprinting.org [me daba "perfectly"](http://openprinting.org/show_printer.cgi?recnum=Samsung-SCX_4200) como calificación de su funcionamiento sobre Linux...

En resumen, después de leer todos esos links, esperaba encontrar:

- Los drivers del CD son "viejos". Bajar de la página de [Samsung](http://www.samsung.com.ar/) la última versión.
- En Ubuntu, como el shell por defecto es [Dash](http://en.wikipedia.org/wiki/Debian_Almquist_shell) ("/bin/sh" apunta a "/bin/dash"), había que cambiar la primera línea del script de instalación de drivers de "/bin/sh" a "/bin/bash"; el script contenía una instrucción ("source" creo), que no funciona bien en Dash y sí en [Bash](http://en.wikipedia.org/wiki/Bourne_again_shell).
- La impresión iba a funcionar sin problemas. Era preferible usar el dispositivo file:/dev/lp0 en vez de file:/dev/mfp4.
- Samsung instala un [módulo](http://en.wikipedia.org/wiki/Linux_Kernel_Module) propietario de kernel, llamado MFP (MultiFunction Port), para poder conectar varios "dispositivos" a un port, pero era preferible evitar usarlo porque era innecesario (ver los links; igualmente era sólo cambiar el puerto en la configuración de [CUPS](http://www.cups.org/)).
- El [XSane](http://www.xsane.org/) (programa de escaneo), sólo iba a permitir escanear como usuario root. El problema estaba en que este módulo MFP ejecutaba una instrucción de "detección" de dispositivos sobre el [puerto paralelo](http://en.wikipedia.org/wiki/Parallel_port) que requería permisos de root; es por eso que el instalador seteaba a XSane como [setuid](http://en.wikipedia.org/wiki/Setuid). Como no necesito nada en el puerto paralelo (es todo [USB](http://en.wikipedia.org/wiki/Usb)), instalé [ésta versión de las librerías](http://jacobo.tarrio.org/Samsung_SCX-4200_on_Debian) modificadas para "saltearse" la parte de detección de los puertos paralelos y escanear como usuario no-root.

Acá hay una captura de la utilidad de administración de impresora, escáner y puertos de Samsung para Linux (basada en [QT](http://www.trolltech.com/products/qt)):

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/ReOd_YVHRwI/AAAAAAAAADU/wCKwkTT4GJQ/s400/samsung-admin.jpg)](http://2.bp.blogspot.com/_nDZ247g0qSM/ReOd_YVHRwI/AAAAAAAAADU/wCKwkTT4GJQ/s1600-h/samsung-admin.jpg)

En definitiva, si bien había que laburar algo... me parecía que la intención de Samsung era buena (aunque no liberara los drivers), y revisando un poco me entero que [HP no colabora en nada](http://foo2zjs.rkkda.com/) ("And if you work for **Konica Minolta** or **HP**, you **should be embarrased** for your company. Neither company has ever offered any kind of support for this driver.") con la creación de los drivers para sus impresoras más "baratas".

Una HP "barata" (una [1018](http://h10010.www1.hp.com/wwpc/es/es/ho/WF05a/6459-6609-6611-6611-6643-12370252.html) o una [1020](http://h10010.www1.hp.com/wwpc/es/es/sm/WF05a/6459-6609-6611-6611-6643-11597423.html), bastante mejor) era la otra opción al momento de buscar impresora láser para comprar, pero ni siquiera eran multifunción... como me parecía piola tener una fotocopiadora + escáner por la misma plata que la 1020 por ejemplo, compré una Samsung.

Todo funcionó (casi) como esperaba. Cambié el shell del script y el instalador anduvo y la impresión funcionó de una. Seguí las instrucciones de los posts/sitios que había encontrado, pero al momento de escanear con el XSane... obtenía un error fatal: "I/O Error". Y se detenía el escaneo.

Eso no estaba en ningún lado. Busqué, probé, hice mil cosas, pensando que era algún paso que no había seguido... hasta que me puse a mirar el syslog. Encontré esto:

```
rmmod: ERROR: Module mfpportprobe does not exist in /proc/modules
rmmod: ERROR: Module mfpport does not exist in /proc/modules
rmmod: ERROR: Module ppdev does not exist in /proc/modules
kernel: [  226.781867] parport0: PC-style at 0x378 (0x778), irq 7, dma 3 [PCSPP,TRIST
ATE,COMPAT,ECP,DMA]
kernel: [  226.849499] lp0: using parport0 (interrupt-driven).
rmmod: ERROR: Module ppdev does not exist in /proc/modules
kernel: [  226.930894] parport0: PC-style at 0x378 (0x778), irq 7, dma 3 [PCSPP,TRIST
ATE,COMPAT,ECP,DMA]
kernel: [  227.019155] lp0: using parport0 (interrupt-driven).
kernel: [  234.294029] drivers/usb/class/usblp.c: usblp0: nonzero read/write bulk status received: -71
kernel: [  234.325509] drivers/usb/class/usblp.c: usblp0: error -71 reading from printer
kernel: [  234.325796] drivers/usb/class/usblp.c: usblp0: nonzero read/write bulk status received: -71
kernel: [  234.385447] drivers/usb/class/usblp.c: usblp0: error -71 reading from printer
kernel: [  234.385518] drivers/usb/class/usblp.c: usblp0: nonzero read/write bulk status received: -71
kernel: [  234.443150] drivers/usb/class/usblp.c: usblp0: error -71 reading from printer
```

Busqué por errores parecidos... y en [este post](http://www.ussg.iu.edu/hypermail/linux/kernel/0402.3/1488.html) encontré una sugerencia acerca de "desactivar" USB 2.0, descargando el módulo ehci\_hcd, para que las transferencias se hagan sobre USB 1.1....

Y Funcionó! :-D

Conclusiones:

1. Parece que por algún motivo el kernel de Ubuntu Edgy 6.10 para AMD64 (2.6.17.x) en mi mother (NForce 4 Ultra), tiene problemas con USB 2.0 al escanear con una Samsung SCX-4200.
1. Siempre miren los logs!
1. Los drivers costaron instalarlos un poco, pero el aparato funciona de maravillas, lo recomiendo; en mi opinión es un buen balance entre funcionalidades y costo (me salió un poco menos de $550 - pesos argentos, claro).
1. Recomiendo esta impresora! (ya lo dije, no?) :-P

Saludos!
Marcelo
