---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2018-01-31T13:06:39+00:00"
guid: https://blog.marcelofernandez.info/?p=1523
title: CORE Network Emulator
url: /2018/01/core-network-emulator/

---
Investigando herramientas de simulación/emulación de redes con fines educativos, [un amigo](http://bitnegro.blogspot.com.ar/) pasó [esta página](http://www.brianlinkletter.com/open-source-network-simulators/), bastante nutrida por cierto, con un listado de simuladores/emuladores de red, recomendando probar [CORE](https://www.nrl.navy.mil/itd/ncs/products/core).

C.O.R.E., acrónimo de _Common Open Research Environment_, fue un proyecto inicialmente de Boeing (sí, la de los aviones) y que ahora es sponsoreado por el [Laboratorio de Investigación Naval de los Estados Unidos](https://www.nrl.navy.mil/). Le dediqué un rato a revisar qué tal funcionaba, y aquí está lo que pude probar.

A priori, resulta que la última versión (4.8) está en los repos de Ubuntu 16.04, por lo que fue fácil la instalación \[1\]:

```
marcelo@marcelo-notebook:~$ apt-cache search core-network
 core-network - intuitive network emulator that interacts with real nets (metapackage)
 core-network-daemon - intuitive network emulator that interacts with real nets (daemon)
 core-network-gui - intuitive network emulator that interacts with real nets (GUI)

```

Se instalan esos paquetes y listo el pollo, las dependencias son básicas (TCL/TK y quagga):

```
marcelo@marcelo-notebook:~$ sudo apt install core-network core-network-gui
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias
Leyendo la información de estado... Hecho
Se instalarán los siguientes paquetes adicionales:
 core-network-daemon libev4 libtcl8.5 libtk-img libtk8.5 quagga tcl8.5 tk8.5
Paquetes sugeridos:
 libtk-img-doc snmpd tcl-tclreadline
Se instalarán los siguientes paquetes NUEVOS:
 core-network core-network-daemon core-network-gui libev4 libtcl8.5 libtk-img libtk8.5 quagga tcl8.5 tk8.5
0 actualizados, 10 nuevos se instalarán, 0 para eliminar y 2 no actualizados.
Se necesita descargar 3.905 kB de archivos.
Se utilizarán 16,4 MB de espacio de disco adicional después de esta operación.
¿Desea continuar? [S/n]

```

Usarlo es tanto como ejecutar "core-gui" y empezar a armar el mapa de la red tal como se ve en el video de la página que citó Mauro:

{{< figure align=aligncenter width=600 src="/wp-content/uploads/2018/01/core1.png" alt="" >}}

El direccionamiento lo hace automáticamente (aunque es configurable, haciéndole doble click a cada nodo):

[![](/wp-content/uploads/2018/01/core2.png)](/wp-content/uploads/2018/01/core2.png)[![](/wp-content/uploads/2018/01/core3.png)](/wp-content/uploads/2018/01/core3.png)

... y la configuración del ruteo se hace sola (ver más abajo cómo), por lo que luego de armar esa red sencilla, ambos hosts ("PC" y "Server") se ven automáticamente.

Luego de armar la red, se le da al botón de "Play" y se ejecuta todo. Todos los nodos son Linux (nada de emulación routers Cisco ni nada como sucede con [GNS3](https://www.gns3.com/)), y acá viene lo interesante:

- Todos son containers ( [LXC](https://linuxcontainers.org/)) del mismo host de uno, pero **sin estar en un chroot** (?!?),
- Es extremadamente rápido gracias a LXC, pero "raro" ya que todo corre en el mismo _filesystem_ de la máquina de uno (puedo ir a mi home directamente desde cada nodo).
- Dado que es un container, todos los kernels guest son los mismos del host.
- ¡El software que se ejecuta es el mismo del host! Es decir, si digo que este nodo va a tener Apache (servicio "HTTP"), hay que instalar el paquete apache2 en el host, no en el guest. No hay imagen de máquina virtual ni nada por el estilo como pasa con otros emuladores.
- El directorio que te abre al entrar a cada guest es `/tmp/pycore.<PID>/<nombredelhost>.conf/`
- La configuración de los servicios/apps que va a usar en cada host la genera el entorno antes de darle "Play" en `/tmp/pycore.<PID>/<nombredelhost>.conf/`, por ejemplo:

[![](/wp-content/uploads/2018/01/core4.png)](/wp-content/uploads/2018/01/core4.png) Uno puede ir y editar los archivos en el directorio `etc.apache2/` que generó el sistema una vez que se le dio ejecución, o puede hacerlo antes, tocando en el botón "Services" de la configuración del host: [![](/wp-content/uploads/2018/01/core5-300x159.png)](/wp-content/uploads/2018/01/core5.png)

Y luego en el ícono de llave inglesa: [![](/wp-content/uploads/2018/01/core6.png)](/wp-content/uploads/2018/01/core6.png)
Este es un ps desde el guest Server:

```
root@Server:/tmp/pycore.34211/Server.conf# ps faxu
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
root 1 0.0 0.0 9676 1708 ? S 12:20 0:00 /usr/sbin/vnoded -v -c /tmp/pycore.34211/Server -l /tmp/pycore.34211/Server.log -p /tmp/pycore.34211/Server.pid -C /tmp/pycore.34211/Server.conf
root 46 0.0 0.0 65508 3096 ? Ss 12:20 0:00 /usr/sbin/sshd -f /etc/ssh/sshd_config
root 52 0.0 0.0 56680 3772 ? Ss 12:20 0:00 /usr/sbin/apache2 -k start
www-data 53 0.0 0.0 411412 3504 ? Sl 12:20 0:00 \_ /usr/sbin/apache2 -k start
www-data 54 0.0 0.0 411412 3504 ? Sl 12:20 0:00 \_ /usr/sbin/apache2 -k start
root 110 0.0 0.0 23848 3968 pts/19 Ss 12:20 0:00 /bin/bash
root 236 0.0 0.0 39932 3316 pts/19 R+ 12:22 0:00 \_ ps faxu
root@Server:/tmp/pycore.34211/Server.conf#
```

Este es el `running-config` generado por core-network de un router (r1), entrando a Quagga con vtysh: [![](/wp-content/uploads/2018/01/core7.png)](/wp-content/uploads/2018/01/core7.png) Así que ahí vemos que define OSPF automáticamente:
[![](/wp-content/uploads/2018/01/core8-300x191.png)](/wp-content/uploads/2018/01/core8.png)

Respecto a si se pueden guardar/cargar laboratorios completos, sí se puede, en el formato "imn" (ya que CORE es un fork de [Imunes](http://www.imunes.net/), y heredó su formato). Algo muy interesante es que se puede configurar el QoS de cada link, haciéndole doble click a cada línea (no lo probé):

[![](/wp-content/uploads/2018/01/core9.png)](/wp-content/uploads/2018/01/core9.png)

Y que te grafica el ancho de banda en tiempo real de la red: [![](/wp-content/uploads/2018/01/core10-300x219.png)](/wp-content/uploads/2018/01/core10.png)

Por último, para hacer capturas de tráfico es bastante sencillo, probé dos opciones:

1. Se corre en cada nodo que se desea un `tcpdump` guardando el tráfico, queda en el "home" de cada container guest ( `/tmp/pycore.<PID>/<nombredelhost>.conf/`), y uno desde el host directamente ejecuta [Wireshark](https://www.wireshark.org/) y lo abre.
1. Pude guardar todo el tráfico de la red en una única captura (esto, con [netkit](http://wiki.netkit.org/index.php/Main_Page), no lo pudimos hacer de forma directa). Para ello, primero se arranca el entorno de simulación; esto hace que se creen dinámicamente los bridges a nivel de host y las interfaces virtuales de cada container, adjuntas a cada bridge (el virbr0 de la captura es de Virtualbox, ignórenlo):

[![](/wp-content/uploads/2018/01/core11.png)](/wp-content/uploads/2018/01/core11.png)

Y bueno, para hacer la captura de toda la red al mismo tiempo, hay que decirle a Wireshark que capture en todos los bridges: [![](/wp-content/uploads/2018/01/core12-300x148.png)](/wp-content/uploads/2018/01/core12.png) [![](/wp-content/uploads/2018/01/core13-300x165.png)](/wp-content/uploads/2018/01/core13.png)

Filtré porque aparece tráfico OSPF todo el tiempo, lógicamente. Es cuestión de desactivarlo por defecto (configurando el servicio Quagga/Zebra). Un detalle de capturar así es que hay que ordenar por tiempo, porque el _packet number_ queda desordenado. Pero ordenando por tiempo, lo seguí y aparentemente (hice un ping, nada más) el orden se mantiene (el "reloj" sería el host).

El manual está acá, parece que se pueden hacer muchísimas cosas más (entornos distribuidos, scripting automatizado en Python, etc.):
[https://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/index.html](https://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/index.html)

Parece ser una muy buena herramienta para simular entornos de red, practicar y aprender sobre protocolos.

\[1\] Resulta que hace poco lo sacaron de Debian/Ubuntu, porque claro, el entorno gráfico se ejecuta como usuario normal, pero al abrir la consola de cualquier nodo que uno creó entra a la VM como root (y recuerden que no se está dentro de un chroot, con lo cual es root en el host con acceso al filesystem):

[https://github.com/coreemu/core/issues/117](https://github.com/coreemu/core/issues/117)

No lo solucionaron, entonces Debian los sacó, por ende no está en Debian Stretch. Igual [maintainer del paquete](http://eriberto.pro.br/site/) tiene un repositorio personal para Debian/Ubuntu:

[http://eriberto.pro.br/core/](http://eriberto.pro.br/core/)

Saludos
