---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2008-11-10T10:30:00+00:00"
guid: https://blog.marcelofernandez.info/?p=115
title: (Mi) Migración a Ubuntu 8.10
url: /2008/11/mi-migracion-a-ubuntu-810/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/SRgOizVugjI/AAAAAAAABsM/FxbyIjDuuE0/s320/LaptopUbuntu.jpg)](http://www.ubuntu.com/)(Este es un Post que debería haberse publicado el 31/10/2008 y que por diferentes motivos no pude terminar...)

Bueno, hoy llegó el día... luego de leer [este](https://help.ubuntu.com/community/IntrepidUpgrades) y [este](http://www.ubuntu.com/getubuntu/releasenotes/810) documento, ayer bajé tranquilo por [BitTorrent](http://es.wikipedia.org/wiki/BitTorrent_%28protocolo%29) [las imágenes de Ubuntu 8.10](http://www.ubuntu.com/getubuntu/downloadmirrors#bt) para la arquitectura x86-64 versión [desktop](http://releases.ubuntu.com/8.10/ubuntu-8.10-desktop-amd64.iso.torrent) y [alternate](http://releases.ubuntu.com/8.10/ubuntu-8.10-alternate-amd64.iso.torrent), con la intención de probar la primera como Live-CD primero y luego, si todo iba bien, actualizar mi Ubuntu 8.04 a 8.10 aprovechando la segunda [ISO](http://es.wikipedia.org/wiki/Imagen_ISO), tal como está documentado en la página de ["Cómo Actualizar a Ubuntu 8.10"](http://www.ubuntu.com/getubuntu/upgrading).

Migración:
La prueba con el Live-CD fue rápida e indolora; simplemente comprobé que funcionara normalmente el hardware: video, wi-fi, red, webcam, etc. y no hubo novedades, todo funcionaba! (al igual que con Hardy). Luego, arranqué del disco como siempre y montando la ISO de alternate, comencé [el proceso de actualización](http://www.ubuntu.com/getubuntu/upgrading#Upgrading%20Using%20the%20Alternate%20CD/DVD), desde una terminal:

```
sudo mount -o loop ~/Escritorio/ubuntu-8.10-alternate-.iso /media/cdrom0
sudo /cdrom/cdromupgrade &
```

Arrancó el software de actualización, me preguntó si quería utilizar la conexión a internet para bajar paquetes extra que no estén en el CD y/o paquetes más nuevos, a lo cual acepté y me puse a trabajar mientras esto pasaba en segundo plano.

Luego de un par de horas de bajar a unos ~30 KB promedio\[1\] unos ~300 paquetes (de ~1500 paquetes a actualizar), comenzó el proceso de instalación real de la nueva versión. Ahí paré de para ir a comer (por hambre y por precaución), ya que a medida que iba transcurriendo el proceso, programa nuevo que abría, programa que aparecía nuevo y "feo", seguramente porque arrancaba con la mitad de las cosas instaladas y/o configuradas.

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/SQyF7fBMnNI/AAAAAAAABlM/pqRCt4xERgQ/s400/Pantallazo-Actualizaci%C3%B3n+completa+de+la+distribuci%C3%B3n.png)](http://4.bp.blogspot.com/_nDZ247g0qSM/SQyF7fBMnNI/AAAAAAAABlM/pqRCt4xERgQ/s1600-h/Pantallazo-Actualizaci%C3%B3n+completa+de+la+distribuci%C3%B3n.png)
Cuando vuelvo de comer, me encuentro con un cuadro de diálogo de reemplazo de archivos de configuración (¡Estaría bueno que primero instale el paquete y por último me pregunte todo junto!).
En fin, el actualizador me preguntó si quería reemplazar 3 archivos de configuración del sistema que yo había modificado anteriormente: cups.conf (configuración del [servidor de impresión](http://www.cups.org/)), otroquenomeacuerdo y el archivo de configuración del [Apache](http://httpd.apache.org/). Sobreescribí los dos primeros (ya que con apache trabajo\[2\], no quería que la actualización me lo desconfigurara), y reinicié.

Al reiniciar, todo anduvo bárbaro. ¡Presto!

Problemas (?):
El único "problema" que supongo que tengo es que la performance de mi placa de video Intel cayó mucho, más que nada al renderizar los efectos de Compiz; me alegra saber que no soy el único y que [este bug](https://bugs.launchpad.net/ubuntu/+source/xserver-xorg-video-intel/+bug/252094) ya estaba creado. Por las dudas, y porque mi Wi-Fi Intel 4965 aparece [con algunos problemas conocidos](http://www.ubuntu.com/getubuntu/releasenotes/810#System%20lock-ups%20with%20Intel%204965%20wireless) en las [Release Notes](http://www.ubuntu.com/getubuntu/releasenotes/810) (aunque yo no tuve ninguno), instalé el paquete linux-backports-modules-intrepid y anda mejor (aunque no tan fluído como con Hardy). Espero que con el correr de los días estos problemas se vayan resolviendo a través de actualizaciones.

Conclusión:
Salvo el detalle anterior, el SO sigue siendo tan sólido como con Hardy, no tuve ningún problema real, digamos... aunque para ser sinceros, salvo la actualización del núcleo, Gnome y el [Network Manager](https://launchpad.net/%7Enetwork-manager) (entre otras cositas como [PulseAudio](http://www.pulseaudio.org/) nuevo), no hay muchas cosas nuevas. No hay Python 2.6 ni Openoffice 3.0 por ejemplo, así que todo es casi tan estable como lo fue con Hardy.

Bueno, es todo, espero que tengan una dulce migración. :-)

Saludos
Marcelo

\[1\] Y está bien para mí, hay que considerar que todo el mundo está comiéndose el ancho de banda de los repositorios... además, tanto el sitio como los repositorios no colapsaron (a diferencia de Hardy, por ejemplo), lo cual es bueno.\[2\] De todas maneras, el programa de actualización te deja el archivo de configuración viejo o nuevo (depende si lo sobreescribo o no) en la ubicación destino pero renombrado en su extensión a "dpkg-old" o ".dpkg-dist", respectivamente . Es decir, pueden plantearse dos casos: si sobreescribo el archivo /etc/cups.conf con uno actualizado, el original queda como /etc/cups.conf.dpkg-old. Caso contrario, si le digo que no me sobreescriba mi /etc/apache2/apache2.conf, me queda un /etc/apache2/apache2.conf.dpkg-dist por si quiero ver más tarde qué diferencias había con el mío.
