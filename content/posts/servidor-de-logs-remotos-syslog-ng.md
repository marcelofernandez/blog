---
author: Marcelo
category:
  - codear
  - linux
  - programación
  - python
date: "2006-10-11T12:31:00+00:00"
guid: https://blog.marcelofernandez.info/?p=14
title: Servidor de Logs Remotos - Syslog-ng
url: /2006/10/servidor-de-logs-remotos-syslog-ng/

---
[Syslog-ng](http://www.balabit.com/products/syslog-ng/) es un demonio de syslog infinitamente más flexible que el syslog "común" (tomado de BSD). Permite aplicar filtros, clasificar de acuerdo a distintos orígenes y enviar a diferentes destinos los logs. Para gestionar un servidor de logs, es muy fácil.

Sólo fue cuestión de leer [esta guia](http://www.debian-administration.org/articles/24) y darle "apt-get install syslog-ng" y listo. Esto reemplaza al syslog anterior, pero la configuración del syslog-ng es la misma (en cuanto a funcionalidad, no en cuanto a sintaxis) que la de un debian recién instalado.

Aquí me aparto un poco de la guía, ya que allí se indica cómo se hace para guardar en directorios distintos los logs de los distintos hosts que loguean desde la red.... y yo en realidad necesitaba un poco más que guardar en un archivo, si no que necesitaba almacenarlo en una base de datos.

Por suerte, syslog-ng es lo suficientemente flexible para enviar los logs a un programa externo, con la opción " [destination ( program("..."); );](http://www.balabit.com/products/syslog-ng/reference-1.6/syslog-ng.html/index.html#destinations)", donde "..." es el comando a ejecutar y el log se le pasa por el standard input. La documentación misma dice que no mata el proceso, sino que lo deja corriendo en otro thread y cuando el syslog-ng recibe una SIGTERM o una SIGKILL, recién ahí lo mata.

Tenía toda la info, sólo faltaba escribir un bello poema en Python para cargar los logs en la BD. Además, tenía que responder a las señales de SIGHUP, TERM y KILL para cerrar la conexión a la BD. Esto es básicamente lo que hice:

```
#!/usr/bin/python

import sys, signal
import BDConnection

def handler(signum, frame):
    salir = True

if __name__ == '__main__':
    bd = BDConnection()
    salir = False

    # Programo las seniales a las cuales voy a responder
    signal.signal(signal.SIGHUP, handler)
    signal.signal(signal.SIGTERM, handler)
    signal.signal(signal.SIGKILL, handler)

    try:
        while not salir:
            input_log = sys.stdin.readline() # Acá se queda bloqueado esperando un \n
            bd.insert(input_log.rstrip()) # rstrip vuela el \n
    # El IOError se genera porque interrumpo el bloqueo del readline()
    except IOError:
        pass

    bd.close()
```

Y funciona! De más está decir que hay que implementar la clase BDConnection, pero ustedes ya se dan una idea de cómo es. :-D

Saludos!
