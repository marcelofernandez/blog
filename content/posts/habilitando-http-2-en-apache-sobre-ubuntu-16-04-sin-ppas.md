---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - web
date: "2017-03-14T19:39:58+00:00"
guid: https://blog.marcelofernandez.info/?p=1475
title: Habilitando HTTP/2 en Apache sobre Ubuntu 16.04 sin PPAs
url: /2017/03/habilitando-http2-en-apache-sobre-ubuntu-16-04-sin-ppas/

---
Una de las cosas que me sorprendió de la versión de [Apache 2.4.18 incluida en Ubuntu 16.04](http://packages.ubuntu.com/xenial/apache2) (y también en Ubuntu 16.10) es que [el módulo mod\_http2 no está habilitado por ser considerado "experimental](https://wiki.ubuntu.com/XenialXerus/ReleaseNotes?_ga=1.128555916.1457848077.1479625589#HTTP.2F2_support_in_Apache_httpd)" por el proyecto Apache.

Discusiones de si debe estar o no al margen, esta característica funciona mayormente bien y no tiene fallas importantes. En mi caso evalué un par de alternativas:

- Compilar un server nuevo desde los fuentes es poco práctico, ya que la integración de Apache lograda por el empaquetado de Debian/Ubuntu es mala o dificultosa como mínimo, además de tener que compilar por futuras actualizaciones de seguridad.
- Confiar en un repositorio PPA ajeno (como indican [otros posts](https://www.digitalocean.com/community/questions/enable-http2-in-apache-on-ubuntu-16-04)) también es un problema, más aún bajo ciertos entornos.

Es por esto que, buscando una alternativa diferente, encontré una manera relativamente fácil, rápida y "con actualizaciones" de habilitar el módulo HTTP/2 sobre el mismo Apache 2.4.18 que trae Ubuntu 16.04/16.10.

Dado que el módulo http2 se incluye en los fuentes, es posible compilarlo y copiarlo nuevamente a la instalación de Apache creada por el paquete Ubuntu. Como requisito, hay que tener los repositorios deb-src habilitados en el archivo /etc/apt/sources.list.

Luego se debe instalar libnghttp2-dev, descargar el paquete fuente de apache2 y compilarlo sin hacer ningún cambio. Los comandos para hacer esto son, como usuario con permisos de sudo sobre el sistema:

```
$ sudo apt-get install libnghttp2-dev
$ mkdir apache2
$ cd apache2
$ apt-get source apache2
$ sudo apt-get build-dep apache2
$ cd apache-2.4.18
$ fakeroot debian/rules binary

```

Después, copiar el módulo compilado (mod\_http2.so) al directorio de módulos del apache, crear un archivo .load en /etc/apache2/mods-available y ejecutar a2enmod http2 para que Apache lo cargue. Luego, se reinicia el servicio y listo.

```
$ sudo cp debian/apache2-bin/usr/lib/apache2/modules/mod_http2.so /usr/lib/apache2/modules/
$ cat << EOF > /tmp/http2.load
LoadModule http2_module /usr/lib/apache2/modules/mod_http2.so

  LogLevel http2:info

EOF
$ sudo cp /tmp/http2.load /etc/apache2/mods-available/http2.load
$ sudo a2enmod http2
$ sudo service apache2 restart

```

Y listo. Luego resta configurar algunas de las [opciones del módulo](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) e incorporarlo en los VirtualHosts que necesitemos, indicando "Protocols h2 http/1.1".

Para más info de cómo instalarlo, [leer acá](https://httpd.apache.org/docs/2.4/howto/http2.html).

Saludos
