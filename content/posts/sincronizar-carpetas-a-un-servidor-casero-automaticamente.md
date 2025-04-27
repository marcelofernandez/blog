---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2010-06-18T11:00:46+00:00"
guid: https://blog.marcelofernandez.info/?p=837
title: Sincronizar carpetas a un Servidor Casero automáticamente
url: /2010/06/sincronizar-carpetas-a-un-servidor-casero-automaticamente/

---
## Introducción

Supongamos que tengo un equipo donde usualmente estoy trabajando y otro equipo que está siempre encendido, ambos separados por Internet. En este último, el único puerto abierto sobre una IP Pública es el 22 para usar [SSH](http://www.openssh.com/), con lo cual me viene perfecta la capacidad de [rsync](http://samba.anu.edu.au/rsync/) de sincronizar carpetas y los últimos cambios mientras estoy trabajando, todo mediante un canal seguro.

## El Script de sincronización

Lo único que necesito es este script en la carpeta personal de mi equipo "cliente", o sea, donde hago mis quehaceres diarios:

```
#!/bin/bash
# Script para sincronización a Servidor vía Casero rsync/ssh.
#
# Copyleft 2010 - Licencia BSD
# Autor: Marcelo Fernández.
# Email: marcelo.fidel.fernandez@gmail.com
#
# Características:
#  - Guarda la salida en un archivo de log (~/sync.log).
#  - Sincronización de una vía; pisa las modificaciones (y elimina) en el destino.
#  - Envía notificaciones al escritorio del usuario.
#  - Se sugiere ser llamado desde cron (gnome-schedule para el usuario final).
#  - Sincroniza (una sola vía) el directorio Documentos de mi Carpeta Personal y el Escritorio,
#    se pueden agregar más carpetas a gusto al momento de ejecutar el comando rsync.
#    También debe personalizarse el destino del backup, ahora en /media/Disco1/Backup
#
# Requerimientos:
#  - Rsync
#  - Paquete libnotify-bin (Debian/Ubuntu), que brinda el comando notify-send.
#
# Parámetros:
#  - Host destino. El script se lo llama "./sync.sh mi_host".
#    El login debe ser vía public keys, y la passphrase la maneja Gnome.

HOST=$1
LOG_FILE="sync.log"
export DISPLAY=:0.0 # Para el notify
export SSH_AUTH_SOCK="$(find /tmp/keyring*/ -perm 0755 -type s -user $LOGNAME -group $LOGNAME -name '*ssh' | head -n 1)"

notify-send -u normal --icon=gtk-refresh --category=transfer "Sincronizando a Casa..."
echo `date` >> $LOG_FILE
rsync -avz -e 'ssh' --delete ~/Documentos ~/Escritorio $HOST:/media/Disco1/Backup/ &>> $LOG_FILE
RETVAL=$?
if [ $RETVAL -ne 0 ]; then
    notify-send -u critical --icon=gtk-dialog-error --category=transfer.error  "Error al Sincronizar";
else
    notify-send -u normal --icon=gtk-apply --category=transfer.complete "Sincronización Completa";
fi
```

## Programando su ejecución automática

Este script está pensado y preparado para ser llamado desde la aplicación " [Tareas Programadas](http://gnome-schedule.sourceforge.net/)" de Gnome (disponible en el Centro de Software de Ubuntu o se instala haciendo click [aquí](apt://gnome-schedule)):

[![](/wp-content/uploads/2010/06/Pantallazo-300x72.png)](/wp-content/uploads/2010/06/Pantallazo.png)

Este es el detalle de la configuración de la tarea:

 [![](/wp-content/uploads/2010/06/Pantallazo-3-300x283.png)](/wp-content/uploads/2010/06/Pantallazo-3.png)

Y además nos avisa de que está trabajando y si tuvo éxito o no al sincronizar:

[![](/wp-content/uploads/2010/06/Pantallazo-2.png)](/wp-content/uploads/2010/06/Pantallazo-2.png) [![](/wp-content/uploads/2010/06/Pantallazo-1.png)](/wp-content/uploads/2010/06/Pantallazo-1.png) [![](/wp-content/uploads/2010/06/Pantallazo-4.png)](/wp-content/uploads/2010/06/Pantallazo-4.png)

## Algunas cuestiones para destacar

Como se aclara en los comentarios, cada vez que rsync copia las modificaciones al servidor, "pisa" lo que había anteriormente, lo que comúnmente se denomina sincronización de archivos " [de una vía](http://en.wikipedia.org/wiki/File_synchronization)".

Lo más interesante es que a pesar de ser ejecutado desde Cron, aprovecha las llaves SSH desbloqueadas por Gnome en la sesión de escritorio del usuario, es decir que la primera sincronización, si previamente no me había conectado al equipo remoto, me pide la frase de paso para desbloquear la clave privada; a partir de ese momento ésta queda compartida, gracias al [ssh-agent](http://es.wikipedia.org/wiki/SSH-Agent) que utiliza Gnome en _background_. Luego, mientras siga la sesión de escritorio establecida, va a aprovechar la llave desbloqueada por el usuario y hacer la sincronización sin mayor problema. **Esto es mucho mejor en cuanto a seguridad que usar una clave privada sin frase de paso contra el servidor, y me permite desbloquearla sólo la primera vez y no cada vez que se hace la sincronización, siendo un perfecto balance (al menos para mí) entre seguridad, automatización y comodidad**. Lógicamente esto sirve para utilizar en cualquier conexión SSH que se quiera establecer, y la solución, después de dar muchas vueltas y probar muchas alternativas (¡el ssh del cron no "veía" al agente!), la encontré en [este post](http://www.codealpha.net/163/cron-ssh-and-rsync-and-key-with-passphrase-ubuntu/).

## Conclusión

Este script fue creciendo desde algo muy simple, totalmente manual y que ejecutaba "cada vez que me acordaba" a mejorarlo un poco en cuanto a la automatización y hacerlo más "lindo" como está hoy. Si bien hay muchísimos mecanismos más robustos o con más facilidades (usando los _snapshots_ de rsync por ejemplo y/o sincronizando ida y vuelta), **éste es el que funciona para mí**. Es bien claro lo que hace y no hay ninguna "magia" en el medio; trabaja sin que me moleste en mi actividad diaria y me despreocupo totalmente de que _tenía_ que hacer backup. :-)

Espero que les sirva como a mí. [Acá hay mucha más información](http://www.vicente-navarro.com/blog/2008/01/13/backups-con-rsync/) sobre rsync y una explicación más profunda de las diferentes opciones y alternativas, que por supuesto se pueden utilizar con esto como base.

¡Saludos!

## Actualización - Ubuntu 11.10

En Ubuntu 11.10 hay que tocar levemente la línea donde define la variable SSH\_AUTH\_SOCK, cambia esto:

```
export SSH_AUTH_SOCK="$(find /tmp/keyring*/ -perm 0755 -type s -user $LOGNAME -group $LOGNAME -name '*ssh' | head -n 1)"

```

por esto, reemplazar $USER por el nombre de usuario ("marcelo" en mi caso):

```
export SSH_AUTH_SOCK="$(find /tmp/keyring*/ -type s -user $USER -group $USER -name '*ssh' | head -n 1)"

```

## Actualización - Ubuntu 12.10

En Ubuntu 12.10 hay que tocar levemente la línea donde define la variable SSH\_AUTH\_SOCK, cambia esto:

```
export SSH_AUTH_SOCK="$(find /tmp/keyring*/ -perm 0755 -type s -user $LOGNAME -group $LOGNAME -name '*ssh' | head -n 1)"

```

por esto, reemplazar $USER por el nombre de usuario ("marcelo" en mi caso):

```
export SSH_AUTH_SOCK="$(find /run/user/$USER/keyring*/ -type s -user $USER -group $USER -name '*ssh' | head -n 1)"

```
