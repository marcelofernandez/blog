---
author: Marcelo
category:
  - codear
  - linux
  - ubuntu-ar
date: "2006-11-06T19:11:00+00:00"
guid: https://blog.marcelofernandez.info/?p=20
title: Tu /dev/parport0 no existe? (Ubuntu 6.10)
url: /2006/11/tu-devparport0-no-existe-ubuntu-610/

---
Buiinas...

Hoy, queriendo imprimir dentro de una máquina virtual de [VMWare](http://www.vmware.com/products/home.html) con un Windows XP (no pregunten por qué semejante masoquismo! \[1\]), sobre mi flamante Ubuntu 6.10 Edgy, me encuentro que VMWare no puede usar"/dev/parport0".

"Ja!" Pensé, porque sabía que VMWare no puede utilizar el puerto paralelo mientras [CUPS](http://www.cups.org/) está corriendo y mientras el módulo 'lp' está cargado. Entonces hice "sudo /etc/init.d/cupsys stop" y "sudo rmmod lp" para detener el servicio de impresión y el módulo de impresión respectivamente. Después de eso, volví a arrancar el VMWare. Pero ahora el mensaje cambió, y ahora VMWare decía que "/dev/parport0 no existe"... "Eh? - Ahora sí que no sé qué pasa".

Como esto sí funcionaba sobre Dapper, googlié por algún tipo de problema al respecto, y encontré esto:

[http://www.murrayc.com/blog/permalink/2005/12/24/wheres-my-parport0/](http://www.murrayc.com/blog/permalink/2005/12/24/wheres-my-parport0/)  
[http://linux.wordpress.com/2006/05/29/running-vmware-workstation-55-on-suse-101/](http://linux.wordpress.com/2006/05/29/running-vmware-workstation-55-on-suse-101/)

Y el más definitivo bug:  
[https://launchpad.net/distros/ubuntu/+source/gnome-cups-manager/+bug/29050](https://launchpad.net/distros/ubuntu/+source/gnome-cups-manager/+bug/29050)

Evidentemente, en algunos casos, parece que aunque uno tenga conectada una impresora de puerto paralelo, Ubuntu 6.10 no crea el archivo que representa ese puerto ("/dev/parport0"). Simplemente lo solucioné haciendo un "sudo modprobe ppdev" primero para verificar que VMWare encontraba el puerto paralelo. Y como pude imprimir desde la máquina virtual, agregué la línea "ppdev" (sin comillas) a mi /etc/modules, con el comando "sudo gedit /etc/modules".

Listo, VMWare puede encontrar mi puerto paralelo.

\[1\] Cualquier trámite que quieran hacer varias entidades del Estado Argentino [(AFIP,](http://www.afip.gov.ar/) [Rentas de Buenos Aires](http://www.rentas.gba.gov.ar/)...) requiere un aplicativo llamado [SIAP,](http://www.afip.gov.ar/programas/siap_main.asp) que está hecho con \*puaj\* VB6+Jet DB, mejor conocido como Access \*puaj\* y lamentablemente disponible sólo para plataformas Windows.

Saludos  
Marcelo
