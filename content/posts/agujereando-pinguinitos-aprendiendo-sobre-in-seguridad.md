---
author: Marcelo
category:
  - codear
  - linux
  - tests
date: "2007-03-06T12:37:00+00:00"
guid: https://blog.marcelofernandez.info/?p=47
title: Agujereando Pingüinitos - Aprendiendo sobre (in)Seguridad
url: /2007/03/agujereando-pinguinitos-aprendiendo-sobre-inseguridad/

---
[![](http://1.bp.blogspot.com/_nDZ247g0qSM/RepEwKvI69I/AAAAAAAAAEg/V5S58YhaIc4/s400/tux_picrate_touko_tux2.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/RepEwKvI69I/AAAAAAAAAEg/V5S58YhaIc4/s1600-h/tux_picrate_touko_tux2.png) Disclaimer:  
Esto lo escribí hace tiempo y ahora lo "hago público", ya que me costó algunas horas de laburo hacerlo. Sin embargo, [en esta semana leo](http://www.linux.com/article.pl?sid=07/02/15/1747220) con alegría que alguien va a hacerme ahorrar laburo: la nota sobre la distro [Damn Vulnerable Linux](http://www.damnvulnerablelinux.org/). Básicamente es un [LiveCD](http://es.wikipedia.org/wiki/Live_CD) que trae un Kernel Linux + herramientas GNU altamente vulnerables, con el objetivo de hackearlas, de diversas maneras. Hasta trae documentación con ejemplos de hacking!

Ahora, a lo que escribí (20/01/2007):

Cómo instalar Red Hat 6.2 en un VMWare

Dado que estoy haciendo un laburo teórico/práctico sobre seguridad, me interesaba instalar alguna distro vieja de Linux (como [Red Hat](http://www.redhat.com/) [6.2](http://www.redhat.com/docs/manuals/linux/RHL-6.2-Manual/install-guide/)), pero sobre una máquina virtual, como [VMWare](http://www.vmware.com/).

Los pasos que seguí fueron los siguientes:

\- Quise instalar el Red Hat 5.2 en VMWare. Todo bien, pero en el mismo instalador, al querer instalar un paquete rpm, me daba errores porque no era de la misma arquitectura. Se ve que rpm validaba la arquitectura que tengo (VMWare le debe decir "x86\_64" y no hace match con "x86"), entonces no instalaba (tengo un Ubuntu 6.10 para AMD64). Es algo parecido a lo que pasa cuando querés instalar un .deb de ubuntu x86 en el Ubuntu de 64 bits y hay que pasarle "--force-architecture" al dpkg. La diferencia está en que no podía hacer que el instalador de RH 5.2 ignorara la arquitectura donde estaba corriendo.

\- Cansado de querer instalar algún RH viejo en VMWare (había intentado con el 6.2 y el 7.0 antes), probé instalarlo sobre [QEmu](http://fabrice.bellard.free.fr/qemu/). Fue tanto como [aptguetearlo](http://es.wikipedia.org/wiki/Advanced_Packaging_Tool), crear una imagen (con /usr/bin/qemu-img) ybootear (con /usr/bin/qemu directamente). Pero en vez de probar el Red Hat 5.2 (que es demasiado viejo ya), volví a probar con el Red Hat 6.2 (que tiene más o menos la antigüedad que quiero).

\- El RH 6.2 instaló perfecto en el QEmu! (a diferencia de VMWare, que [el instalador mostraba un bug](http://www.vmware.com/community/thread.jspa?messageID=438103%F1%AA%BD%97)... documentado para micros Pentium 4 en adelante).

\- Una vez instalado en el QEmu, quería configurar la interfaz de red virtual, para poder acceder a internet y acceder al Ubuntu que lo está corriendo (para hacer pruebas); pero se complicó, ya que si bien al hacer un ifconfig veía la interfaz (Qemu simula tener una placa trivial NE2000), se me complicó porque para hacer andar la red con QEmu hay que simular una interfaz TUN (algo parecido a lo que hicimos con el OpenVPN), pero probé un ratito y como no anduvo, colgué el QEmu.

\- Me acordé que el mismo programa de manajo de imágenes de disco (qemu-img) de QEmu soporta el formato de VMWare... así que convertí la imagen del RedHat 6.2 ya instalado en una imagen de disco de VMWare!!!

\- Creé una nueva máquina virtual de VMWare y le puse como disco rígido la imagen convertida desde QEmu. Todo anduvo a la perfección; el Kudzu del RH 6.2, corriendo adentro del VMWare me reconoció el "nuevo" hardware (el hardware que emula QEmu es distinto al de VMWare) y listo el pollo. Tengo RedHat 6.2 en el VMWare!

\- Hice ping, salgo a internet y todo joyita. Ahora queda hacerlo pelota.

Saludos  
Marcelo  
PD: Créditos a [The Hackademy](http://www.thehackademy.net/) por el Tux hermoso de este post. :-D
