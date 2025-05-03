---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2010-02-27T19:55:25+00:00"
guid: https://blog.marcelofernandez.info/?p=538
title: Virtio - Paravirtualización de I/O
url: /2010/02/virtio-paravirtualizacion-de-io/

---
[![](/wp-content/uploads/2010/02/kvmbanner-logo2.png)](http://www.linux-kvm.org) Hace un rato que estoy leyendo sobre virtualización, pero no de CPU, sino de [I/O](http://arstechnica.com/business/guides/2010/02/io-virtualization.ars/) (otro más [acá](http://www.ibm.com/developerworks/linux/library/l-virtio/index.html?ca=dgr-lnxw07Viriodth-LX&S_TACT=105AGX59&S_CMP=grlnxw07))... muy interesante, me aclaró algunas dudas que tenía, dado que últimamente al configurar este tipo de software se me confundían las cosas :-)

Resulta que a nivel de I/O tenemos algo parecido a la virtualización al nivel de CPU: emulación, paravirtualización y ejecución "directa", por llamarlo de alguna manera. Sugiero leer el artículo para más detalles, pero sólo quiero agregar que recién estamos en la etapa de paravirtualización, y que (en buena hora) Intel y AMD agregaron unidades [IOMMU](http://en.wikipedia.org/wiki/IOMMU) en sus últimos diseños para poder asignar dispositivos directamente a una (o más, según el caso) VM, evitando el Hypervisor y ahorrando ciclos de CPU. Pero esto parece estar verde aún.

[Virtio](http://wiki.libvirt.org/page/Virtio) vendría a ser como una serie de drivers de paravirtualización de I/O. Lo bueno es que no sabía que [VirtualBox](http://www.virtualbox.org/) [soportaba](http://www.virtualbox.org/manual/ch06.html#nichardware) para las interfaces de red, algo que al parecer (y de esta manera) intenta ser un estándar  (net y block devices por ahora),  ya que tiene una API de comunicación Host <--> Guest abierta, independiente y en paralelo a la propuesta de [KVM](http://www.linux-kvm.org/page/Main_Page) como hypervisor.

Luego de ver un poco todo esto, me puse  a configurarlo en una VM con WinXP, le instalé [estos drivers](http://www.linux-kvm.org/page/WindowsGuestDrivers/Download_Drivers) y ya estoy usando menos CPU para la parte de red en mi guest :-)

[![](/wp-content/uploads/2010/02/Pantallazo-Windows-XP-Configuración-300x201.png)](/wp-content/uploads/2010/02/Pantallazo-Windows-XP-Configuración.png)

Acá se ve un poco la mejora a nivel de tráfico de red \*real\* que hay usando I/O paravirtualizado:

[http://www.linux-kvm.org/page/Using\_VirtIO\_NIC](http://www.linux-kvm.org/page/Using_VirtIO_NIC)

En fin, esperemos que todo esto mejore más aún. KVM ya soporta virtio para [dispositivos de almacenamiento](http://www.linux-kvm.com/content/redhat-54-windows-virtio-drivers-part-2-block-drivers), además de [los de red](http://www.linux-kvm.com/content/tip-how-setup-windows-guest-paravirtual-network-drivers) (linkeo la instalación de los drivers de Windows porque lógicamente Linux tiene estas baterías ya incluídas, je).

Saludos
