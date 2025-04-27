---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2011-01-05T10:47:10+00:00"
guid: https://blog.marcelofernandez.info/?p=1037
title: Comparativas de Virtualización KVM
url: /2011/01/comparativas-de-virtualizacion-kvm/

---
Ultimamente por diferentes cuestiones personales estoy bastante desconectado de la lectura de noticias, blogs y demás. Sin embargo, me pareció útil dejar por acá dos comparativas sobre KVM de [Phoronix.com](http://www.phoronix.com/), que si bien en algunos lados me encontré que se quejan de la "calidad" de su suite de [benchmarks](http://www.phoronix-test-suite.com/) para software libre/abierto, por el otro, ¡hey! ¡al menos se preocupan en hacerlo! :-)

**[Linux KVM vs. VirtualBox 4.0 Virtualization Benchmarks](http://www.phoronix.com/scan.php?page=article&item=linux_kvm_virtualbox4&num=1)**
Este es un test de performance entre [KVM](http://www.linux-kvm.com/) y [Virtualbox](http://www.virtualbox.org/). En resumen, y con los parámetros por defecto en cuanto a I/O, VBox gana, pero parece que es debido a que no hace los [fsync](http://pubs.opengroup.org/onlinepubs/007908799/xsh/fsync.html) del guest en el host y termina utilizando su caché; muy probablemente este comportamiento es configurable (¿ver [acá](http://www.virtualbox.org/manual/ch05.html#iocaching)?), pero así y todo ¡es bueno saberlo!. De todas maneras, de alguna manera es peligroso para un entorno de servidores ya que hay riesgo de pérdida de datos, pero por ejemplo, para mi notebook ( _target_ primario adonde Virtualbox siempre estuvo orientado) está bien.

Por el otro lado, cuando hay que usar la CPU, KVM "le pasa el trapo" a VBox. Hay un test de I/O de red (TCP) que a KVM le dio mal, tan mal, que seguro es debido a que los salames testers no habilitaron [VirtIO](http://wiki.libvirt.org/page/Virtio#Network_driver) en los guests KVM.

[**Multi-Core Scaling In A KVM Virtualized Environment**](http://www.phoronix.com/scan.php?page=article&item=linux_kvm_scaling&num=1)
Este test trata de dilucidar si es mito o no que el Hyperthreading (las VCPUs) de los microprocesadores [Intel Xeon basados en Nehalem](http://en.wikipedia.org/wiki/Intel_Xeon#Nehalem_based_Xeon) hacen más lentas o más rápidas las VMs que corren sobre KVM. Esto es algo que pregunté personalmente en el IRC de KVM (#kvm en [Freenode](http://freenode.net/), dicho sea de paso), y me sugirieron lo mismo que dió en los tests: hay veces en donde las VMs aprovechan todas las CPUs virtuales (VCPUs) y otras en que no, con lo cual hay que probar en cada caso en particular y dejar la configuración que nos dé mejores resultados.

Interesante de leer y experimentar.

Saludos
