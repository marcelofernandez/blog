---
author: mfernandez
category:
  - codear
  - sysadmin
  - ubuntu-ar
date: "2008-06-09T10:30:00+00:00"
guid: https://blog.marcelofernandez.info/?p=104
title: FreeBSD y Actualizaciones
url: /2008/06/freebsd-y-actualizaciones/

---
Los muchachos de [Kriptópolis](http://www.kriptopolis.org/) hace algun tiempito que vienen haciendo posts sobre experiencias con BSD. En este caso, postean sobre una de las cuestiones más críticas (para mí) en la administración de un servidor: el manejo de actualizaciones y las vulnerabilidades.

[http://www.kriptopolis.org/actualizar-software-vulnerable-freebsd](http://www.kriptopolis.org/actualizar-software-vulnerable-freebsd)  
[http://www.kriptopolis.org/actualizar-software-vulnerable-freebsd-2](http://www.kriptopolis.org/actualizar-software-vulnerable-freebsd-2)

Una característica muy particular comparado con Debian/Ubuntu es que se recomienda utilizar los ports, a diferencia de los paquetes binarios. Sin embargo, se mantiene la info de dependencias, aunque para hacer un "upgrade" general de un paquete y sus dependencias haya que recurrir a software no instalado por defecto (portmanager, portmaster, portugrade, etc.).

Lo que sí es una delicia es el [portaudit](http://www.freebsd.org/doc/en/books/handbook/security-portaudit.html). Eso sí me gustaría verlo en Debian/Ubuntu ya! Aunque es más bien difícil que se porte, ya que es una de las ventajas de laburar siempre con software mínimamente modificado de origen: no hay vulnerabilidades particulares "de la distro", sino que las vulnerabilidades se obtienen directamente del fabricante o comunidad de desarrollo origen. Ojo, no digo que las listas [DSA](http://www.debian.org/security/) y [USN](http://www.ubuntu.com/usn) tengan menos utilidad, sólo que me gusta "preguntarle" a mi SO directamente de la consola qué paquetes tiene instalados con vulnerabilidades conocidas. :-)

De todas formas, lindos posts para chusmear un rato.

Salutes  
Marcelo
