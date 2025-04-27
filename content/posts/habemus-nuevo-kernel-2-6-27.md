---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2008-10-13T02:14:00+00:00"
guid: https://blog.marcelofernandez.info/?p=111
title: 'Habemus Nuevo Kernel: 2.6.27'
url: /2008/10/habemus-nuevo-kernel-2627/

---
[![](http://2.bp.blogspot.com/_nDZ247g0qSM/SPOIkrrCP5I/AAAAAAAABPU/KBeSP9b3qDQ/s320/officialpenguin.gif)](http://www.linux.org/) Bueno, si bien hace unos días que salió una nueva revisión del [kernel de Linux](http://www.kernel.org/), 2.6.27, yo tenía pendiente leer [los cambios](http://kernelnewbies.org/Linux_2_6_27) (versión "light", o sea, entendible por alguien más o menos técnico) al estilo que me tiene acostumbrado [KernelNewbies](http://kernelnewbies.org/).

Y aunque en el [Blog de Diego Calleja](http://diegocg.blogspot.com/) [ya hay un buen resumen](http://diegocg.blogspot.com/2008/09/lo-que-traer-linux-2627.html) de lo más "grueso" (aka "The Cool Stuff") de esta versión (en castellano), yo quería comentar algunas cosas que están en la "letra chica" del changelog. De nuevo, para lo más importante, los invito a pasar por [el Blog de Diego](http://diegocg.blogspot.com/2008/09/lo-que-traer-linux-2627.html).  

- [Nueva forma de hibernar](http://lwn.net/Articles/242107/): Es un método alternativo, no reemplaza a los anteriores, a menos que se lo indique explícitamente. Además, está sólo disponible para la arquitectura x86, y en principio utiliza mucho del código de hibernación actual, con lo cual los problemas existentes con los distintos drivers de dispositivos van a persistir; sin embargo mucho de éste código problemático no es necesario con este nuevo método, con lo cual en el futuro la hibernación será óptima y sin los problemas de hoy. Es decir, es un primer paso que sienta las bases para futuros desarrollos y mejoras, que van a ir tomando forma en posteriores versiones.  

- [ACPI PCI Slot detection driver](http://git.kernel.org/?p=linux/kernel/git/torvalds/linux-2.6.git;a=commit;h=8344b568f5bdc7ee1bba909de3294c6348c36056): Cuántas veces olvidamos cuántos slots PCI tengo disponibles/ocupados en mi motherboard? Muchas. Y cuántas veces de ésas tengo ganas de abrir la CPU para fijarme? (o ir siquiera si estoy remoto)? Ninguna. Todos estos problemas se esfuman con esta mejora, porque ahora podemos consultarle a Linux la geografía física del subsistema PCI, consultando el (nuevo) directorio /sys/bus/pci/slots.
- Mejoras en Xen: Cuando el kernel corre en [modo "paravirtualizado"](http://wiki.xensource.com/xenwiki/XenParavirtOps) como guest de un kernel Xen en modo "hypervisor", la arquitectura del kernel guest es "Xen" (no por ejemplo "x86" o "x86\_64"). Bueno, hay algunos cambios para esta arquitectura, de relativa importancia: Se permiten hosts de 64 bits, save/restore de la VM, y [algunas cosas más](http://kernelnewbies.org/Linux_2_6_27#head-b14ab696e23019ae4b6362c5816bcd23a55240ce).
- Drivers Wi-Fi intel mejorados: Los drivers en general de estas placas wifi fueron ampliamente mejorados: los módulos iwl4965 y iwl3945 ahora soportan modo monitor (mi placa es iwl4965!), consultarles el nivel de energía, modo [Ad-hoc](http://en.wikipedia.org/wiki/Wireless_ad-hoc_network) y algunas cositas más.

Bueno, éste pequeño listado es lo "extra" que rescaté de la nueva versión del kernel, que será incluído en [Ubuntu Intrepid Ibex 8.10](https://wiki.ubuntu.com/IntrepidIbex). Por ejemplo, ahora sé que con este nuevo kernel en mi máquina voy a poder correr [kismet](http://www.kismetwireless.net/) y escuchar por redes ajenas... :-D

Saludos!  
Marcelo
