---
author: Marcelo
category:
  - codear
  - linux
date: "2006-11-04T01:22:00+00:00"
guid: https://blog.marcelofernandez.info/?p=19
title: Ojo con los Proxys Transparentes de los ISPs
url: /2006/11/ojo-con-los-proxys-transparentes-de-los-isps/

---
Actualización (21/11/2007): Buanzo en [su blog](http://blogs.buanzo.com.ar/) [aporta un método mejor](http://blogs.buanzo.com.ar/2007/11/aptconf-para-fibertel-speedy-y-otros.html) para evitar esto que describo en este post, cambiando la configuración del apt.

Holas, este post es para comentar algo que me pasa de vez en cuando y que me pone los pelos de punta: los [proxys transparentes](http://es.wikipedia.org/wiki/Proxy) de los ISPs ( [Speedy](http://www.speedy.com.ar/) en mi caso). Resulta que como comenté anteriormente, hace poco me actualicé a [Ubuntu](http://www.ubuntulinux.com/) 6.10 y en estos últimos días salieron algunos parches de seguridad (estoy suscripto a la [lista de USN](https://lists.ubuntu.com/mailman/listinfo/ubuntu-security-announce) \- Ubuntu Security News).

Pero no importa lo que hiciera, siempre tenía este error al hacer apt-get update:

```
root@marcelo-desktop:/usr/bin# apt-get update
[...]
Des:10 http://archive.ubuntu.com edgy-proposed Release [19,6kB]
Ign http://archive.ubuntu.com edgy-proposed Release
Obj http://archive.ubuntu.com edgy-proposed/main Packages
Obj http://archive.ubuntu.com edgy-proposed/restricted Packages
Obj http://archive.ubuntu.com edgy-proposed/universe Packages
Obj http://archive.ubuntu.com edgy-proposed/multiverse Packages
Descargados 33,9kB en 5s (6588B/s)
Imposible obtener http://security.ubuntu.com/ubuntu/dists/edgy-security/main/
binary-amd64/Packages.bz2  La suma MD5 difiere
Leyendo lista de paquetes... Hecho
W: GPG error: http://archive.ubuntu.com edgy-proposed Release: Las siguientes
firmas fueron inválidas: BADSIG 40976EAF437D05B5 Ubuntu Archive Automatic Signing Key
W: Tal vez quiera ejecutar 'apt-get update' para corregir estos problemas
E: Algunos archivos de índice no se han podido descargar, se han ignorado,
o se ha utilizado unos antiguos en su lugar.
root@marcelo-desktop:/usr/bin#
```

\- ¿Y esto? - me pregunté. Al principio tenía sólo el problema de "la suma MD5 difiere". Después de seguir intentando (dejé pasar un par de días), se me agregó el tema de que no concordaba la clave pública del repositorio edgy-proposed.

Googlié (así se dice? :-P ), revolví ("apt-get install --reinstall ubuntu-keyring") y nada... hasta que recordé que una vez estuve 2 días esperando que la caché del Proxy Transparente de Speedy se "vaciara" ("flusheara", en mi jerga) para que me muestre actualizados los cambios que estuve haciendo en una página web que estaba desarrollando en ese momento.

Cómo detecte (en ese momento) el problema? Fácil: Acá entran los amigos de uno en juego. Resulta que tenía uno con Arnet, otro con Fibertel y otro con Speedy. Yo había modificado la página, vacié mil veces la caché del navegador, y seguía viendo la página "vieja". Les pedí ayuda a mis amigos, y los de Arnet y Fibertel veían perfectamente las modificaciones que yo había subido hacía minutos... pero mi amigo de Speedy no! (y eso que era la primera vez que él entraba a verla, así que no había caché de navegador que valiese).

Lo que pasaba era que el Proxy (transparente, porque yo no tengo que configurar un proxy en mi conexión para navegar) de mi ISP no me estaba "caducando" ("venciendo") del caché la página, entonces no consultaba el server original y me devolvía una versión "vieja" de la página, desde su almacenamiento local.

Esperé un par de días hasta que la página "caducara" en el Proxy (totalmente fuera de mi control, claro), y magia: solita se actualizó (y eso que probé con los encabezados http para hacerla vencer... pero nada, se ve que el proxy que están utilizando debe tener algún problema, ni idea)...

Bueh, recordando este inconveniente que me sucedió hace un tiempo, me dije: "no será un problema del proxy de Speedy que me está mandando una versión corrupta de los índices de los repositorios?" Entonces, para "burlar" cualquier proxy (y en este caso, el de Speedy), se debe usar otro proxy, je.

Fue tanto como googlear por proxys anónimos (hay bots que escanean la red en busca de proxys mal configurado, sin autenticación, libres de uso) y encontré [una página con Proxys catalogados por país](http://www.samair.ru/proxy/) (tercer link de Google, je). (Ojo, puede que dentro de un tiempo no sirvan...)

Entonces fue tanto como probar al azar las IPs de la lista; uno no me contestaba, el otro me daba 111 - Conexión rehusada, hasta que encontré uno que empezó a bajar los índices del repo de Ubuntu:

Intento 1:

```
root@marcelo-desktop:/usr/bin# export http_proxy=http://201.37.117.34:8080
root@marcelo-desktop:/usr/bin# apt-get update
0% [Conectando a 201.37.117.34 (201.37.117.34)] [Conectando a 201.37.117.34
(201.37.117.34)] [Conectando a 201.37.117.34 (201.37.117.34)] [...]
```

Intento 2:

```
root@marcelo-desktop:/usr/bin# export http_proxy=http://201.21.222.112:6588
root@marcelo-desktop:/usr/bin# apt-get update
Err http://www.kubuntu.org edgy Release.gpg
No pude conectarme a 201.21.222.112:6588 (201.21.222.112). -
connect (111 Conexión rechazada)
Err http://espergreen.com edgy Release.gpg
[...]
```

Intento 3:

```
root@marcelo-desktop:/usr/bin# export http_proxy=http://200.19.159.35:3128
root@marcelo-desktop:/usr/bin# apt-get update
Des:1 http://www.kubuntu.org edgy Release.gpg [189B]
Des:2 http://archive.canonical.com edgy-commercial Release.gpg [191B]
Des:3 http://security.ubuntu.com edgy-security Release.gpg [191B]
Des:4 http://www.amd64.aceracerftw.com edgy Release.gpg [189B]
[...]
Obj http://archive.ubuntu.com edgy-proposed/multiverse Packages
Descargados 26,1kB en 1m35s (273B/s)
Leyendo lista de paquetes... Hecho
root@marcelo-desktop:/usr/bin#
```

¡Joya! Como el destino de la conexión era otra IP distinta de la de los repositorios de Ubuntu, el Proxy de Speedy sí o sí tenía que conectarse a esa IP "desconocida" (que al ser otro proxy me comunicaba con los repos de Ubuntu!). De esta manera, obligo a que no use lo que tiene en su caché.

```
root@marcelo-desktop:/usr/bin# apt-get upgrade
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias
Leyendo información de estado... Hecho
Se actualizarán los siguientes paquetes:
imagemagick irb1.8 libimlib2 libmagick++9c2a libmagick9 libpq4
libreadline-ruby1.8 libruby1.8 libwv-1.2-1
linux-restricted-modules-2.6.17-10-generic
linux-restricted-modules-common nvidia-glx rdoc1.8 ri1.8
ruby1.8 ruby1.8-dev screen wv
18 actualizados, 0 se instalarán, 0 para eliminar y 0 no actualizados.
Necesito descargar 21,4MB de archivos.
Se liberarán 20,5kB después de desempaquetar.
¿Desea continuar [S/n]?
```

Magia!! :-D

Todos (o casi todos) los comandos de consola que utilizan el protocolo HTTP utilizan la variable de entorno ["http\_proxy"](http://ubuntuforums.org/showthread.php?t=1575) para usar proxy en su conexión (aunque podría haber usado el archivo [/etc/apt/apt.conf](http://www.die.net/doc/linux/man/man5/apt.conf.5.html), pero es otra historia). Así fui cambiando de IP de proxy probable hasta que uno me contestó. Y como ven, no me dió ningún error de MD5 que no concuerda, ni de clave pública errónea. Pude burlar al proxy de Speedy! :-D

(Y actualizar mi Ubuntito también, je) :-D

Ojo, que igual era necesario sólo para el 'update'. Al fin y al cabo, bajar los paquetes por medio de un proxy taarda muucho. Al final, una vez que por el proxy bajé los índices y no me dió error, hice un "export http\_proxy=" (sin nada), para volver a la conexión directa y bajar los paquetes sin proxys en el medio.

Así que ya saben: si tienen problemas de refresco/actualización de recursos en la web, chequeen de esta manera. Obviamente es mucho más lento (e inseguro también, ya que podemos sufrir un [MITM Attack](http://en.wikipedia.org/wiki/Man_in_the_middle)) usar un proxy de un tercero que de forma directa, pero... prefiero correr el riesgo y actualizar mi Ubuntu. Después de todo, los paquetes del repo de Ubuntu vienen firmados.

Buenas noches!
Marcelo
