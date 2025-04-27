---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2014-05-16T22:07:05+00:00"
guid: https://blog.marcelofernandez.info/?p=1380
title: Migrando Host de VMs KVM de Ubuntu 12.04 a 14.04
url: /2014/05/migrando-host-de-vms-kvm-12-04-14-04/

---
En estos días arranqué la tarea de migrar Máquinas Virtuales de Hosts Ubuntu 12.04 (entorno QEmu-KVM/Libvirt) a Ubuntu 14.04, para testear que esté funcionando todo ok, y me encontré con dos particularidades:

1) Por defecto, las interfaces de red no se llaman más "ethX" ni se guían como antes con el archivo en /etc/udev/rules.d/70-net-persistent.rules, sino que ahora son medio raras ("p2p1", "p5p4", "eno1", "enp2s0", etc). [Acá está explicado el por qué del cambio](http://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/ "Predictable Network Interface Names") \[1\]. A priori no me gusta nada este cambio, por no respetar ninguna convención como sí lo hacía ethX, y encima por ser un cambio por defecto, pero como todo... supongo que me acostumbraré. Es adaptarse o morir; no es un capricho de Ubuntu, sino de udev.

2) ¡No hace falta más configurar un bridge en el host! Ahora se usa el driver de kernel [macvtap](http://seravo.fi/2012/virtualized-bridged-networking-with-macvtap "Virtualized bridged networking with MacVTap") \[2\]\[3\]\[4\]\[5\], que uno [lo selecciona en el administrador virt-manager](http://www.linux-kvm.com/content/simple-bridged-networking-virt-manager-using-macvtap "Simple Bridged Networking with Virt-Manager using macvtap"). Así que para el host no habría que cambiar la configuración; para recordarlo, en un **host** Ubuntu 12.04 pasábamos de esto:

```
auto eth0
 iface eth0 inet static
 address 192.168.1.18
 netmask 255.255.255.0
 gateway 192.168.1.1
 dns-nameservers 192.168.1.1 192.168.1.2
```

a esto (cambia eth0 por br0, y las líneas de bridge):

```
auto br0
 iface br0 inet static
 address 192.168.1.18
 netmask 255.255.255.0
 gateway 192.168.1.1
 dns-nameservers 192.168.1.10 192.168.1.15
 bridge_ports eth0
 bridge_stp off
 bridge_fd 0
 bridge_maxwait 0
```

Ahora en Ubuntu 14.04 **no hace falta** tocar nada, ya que la interfaz principal sigue siendo la misma, aún cuando haya VMs que salgan directamente puenteadas por dicha interfaz. Lo que sí, por cada VM puenteada con el driver macvtap se va a crear una interfaz en el host, quedando así, digamos:

```
macvtap0 Link encap:Ethernet HWaddr 52:54:00:1e:b5:29
 inet6 addr: fe80::5054:ff:fe1e:b529/64 Scope:Link
 UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
 RX packets:437 errors:0 dropped:0 overruns:0 frame:0
 TX packets:71 errors:0 dropped:0 overruns:0 carrier:0
 collisions:0 txqueuelen:500
 RX bytes:36313 (36.3 KB) TX bytes:6617 (6.6 KB)

 p2p1 Link encap:Ethernet HWaddr 44:87:fc:ef:96:be
 inet addr:192.168.1.18 Bcast:192.168.1.255 Mask:255.255.255.0
 inet6 addr: fe80::4687:fcff:feef:96be/64 Scope:Link
 UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
 RX packets:15346 errors:0 dropped:46 overruns:0 frame:0
 TX packets:16615 errors:0 dropped:0 overruns:0 carrier:0
 collisions:0 txqueuelen:1000
 RX bytes:2096838 (2.0 MB) TX bytes:4714920 (4.7 MB)
```

Esto seguramente es más rápido que lo anterior, por diferentes motivos, pero tiene un lado feo, y es que de esta manera los guests [no pueden comunicarse con el host](http://www.furorteutonicus.eu/2013/08/04/enabling-host-guest-networking-with-kvm-macvlan-and-macvtap/ "Guest can reach outside network, but can't reach host") \[6\]. Apliqué la "less painful solution" de esa wiki, y fue crear una red NAT aislada entre el host y sus guests, todo de manera gráfica desde virt-manager. Fue bastante simple, y aunque no me guste que tenga que haber una interfaz en el guest únicamente para comunicarse con el host, funciona bien y en mi caso esto es para necesidades muy puntuales (emergencia casi siempre).

En resumen, esto fue para darles un pantallazo de algunos de los cambios que vamos a tener que hacer cuando migremos a 14.04. Estos cambios también están en RHEL 7, ya que son todos cambios de proyectos _upstream_, así que no nos ahorramos este trabajo yendo para aquel lado tampoco.

\[1\] [http://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/](http://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/)
\[2\] [http://seravo.fi/2012/virtualized-bridged-networking-with-macvtap](http://seravo.fi/2012/virtualized-bridged-networking-with-macvtap)
\[3\] [http://virt.kernelnewbies.org/MacVTap](http://virt.kernelnewbies.org/MacVTap)
\[4\] [http://libvirt.org/formatnetwork.html#examplesDirect](http://libvirt.org/formatnetwork.html#examplesDirect)
\[5\] [http://www.linux-kvm.com/content/simple-bridged-networking-virt-manager-using-macvtap](http://www.linux-kvm.com/content/simple-bridged-networking-virt-manager-using-macvtap)
\[6\] [http://www.furorteutonicus.eu/2013/08/04/enabling-host-guest-networking-with-kvm-macvlan-and-macvtap/](http://www.furorteutonicus.eu/2013/08/04/enabling-host-guest-networking-with-kvm-macvlan-and-macvtap/ "Enabling host-guest networking with KVM, Macvlan and Macvtap")

Saludos!
