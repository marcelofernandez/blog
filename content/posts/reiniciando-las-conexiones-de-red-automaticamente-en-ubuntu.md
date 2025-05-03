---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2010-06-16T11:00:43+00:00"
guid: https://blog.marcelofernandez.info/?p=816
title: Reiniciando las conexiones de red automáticamente en Ubuntu
url: /2010/06/reiniciando-las-conexiones-de-red-automaticamente-en-ubuntu/

---
[![](/wp-content/uploads/2010/06/ENUWI-G2_pdt_main_090107.png)](http://www.encore-usa.com/ar/product/ENUWI-G2) Hace un tiempo que tengo una interfaz [Wifi USB Encore](http://www.encore-usa.com/ar/product/ENUWI-G2); siempre la usé ocasionalmente, en Ubuntu se conectaba y tenía red sin problemas, pero al momento de usarla en forma constante nunca supuse que el módulo rtl8187, responsable de su funcionamiento, [iba](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/182473) [a](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/225851) [tener](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/293946) [tantos](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/254438) [bugs](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/215802). :-(

En Ubuntu 10.04 funciona sin instalar nada adicional, pero los principales problemas son que funciona sólo a 11Mbps y que aleatoriamente se desconecta, sin causa aparente, y el Network Manager vuelve a reconectarse. El problema es más serio cuando Network Manager se cansa de revivir a la placa, y el equipo se queda desconectado definitivamente. Y más si no estoy cerca para reconectarla a mano. :-P

Luego de probar unas cuantas recetas (y de esquivar Ndiswrapper porque es un equipo de 64 bits), decidí que voy a dejarlo así, y me hice un scriptcito de cron para reiniciar Network Manager si la interfaz está caída:

```
#!/bin/bash

# /usr/bin/network_respawn
# Network respawn -- rtl8187b dies randomly and despite Network-Manager usually
# reconnects again, sometimes it gives up and dies without connection.

# If wlan0 is not connected, it restarts Network-Manager
# This script is meant to be called from a cron job.

IFACE="wlan0"

CONNECTED=`grep $IFACE /var/run/network/ifstate`
if [ "$CONNECTED" == "" ]; then  # Disconnected. Respawning...
    /usr/bin/service network-manager restart
fi
```

Además, hay que crear el archivo en /etc/cron.d/network\_respawn para que el sistema llame al script anterior, cada 3 minutos en mi caso:

```
# Network respawn -- rtl8187b dies randomly and despite Network-Manager usually
# reconnects again, sometimes it gives up and dies without connection.
# If wlan0 is not connected, it restarts Network-Manager

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

*/3 * * * * root /usr/bin/network_respawn
```

Es un script sencillo, y puede ser utilizado para cualquier propósito como el mío. Lo malo es que reinicia todas las conexiones del equipo, ya que reinicia el Network Manager .

¿A alguien se le ocurre cómo hacerlo mejor? :-)

Saludos
