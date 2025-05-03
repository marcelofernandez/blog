---
author: Marcelo
category:
  - codear
  - linux
  - tests
  - ubuntu-ar
date: "2006-10-29T15:22:00+00:00"
guid: https://blog.marcelofernandez.info/?p=17
title: Migré a Ubuntu Edgy!
url: /2006/10/migre-a-ubuntu-edgy/

---
Impresionante, la verdad es que estoy disfrutando de la última actualización a [Edgy.](http://www.ubuntu.com/) Cambié todo lo que decía 'dapper' por 'edgy' en el archivo /etc/apt/sources.list, y ejecuté 'apt-get update', 'apt-get dist-upgrade' y varias horas después tenía Edgy instalado. Me quedaron algunos meta-paquetes de python retenidos, pero no son problema, después lo soluciono.

Mi instalación de [XGL](http://en.wikipedia.org/wiki/Xgl) + [Compiz](http://en.wikipedia.org/wiki/Compiz) anduvo sin romperse, aunque no lo probé mucho porque el Compiz ese (de Quinnstorm, antes de [convertirse](http://www.osnews.com/story.php/15888/Compiz-Forked-Beryl) en Beryl) era para Dapper... ya cambié el repo a Edgy y estoy usando [Beryl](http://www.beryl-project.org/) 0.1.1 para Edgy. Tiene bastantes mejoras, aunque molesta un poco que tenga taaantas opciones yque siempre haya que estar configurando algo porque se actualizó/cambió. Ojo, son paquetes que están en puro desarrollo, así que si te gusta el durazno... bancate la pelusa. :-P

Lo primero que hice fue levantar el [Firefox](http://www.mozilla.com/firefox) 2.0 de 32 bits que tengo (tengo Ubuntu 64) ya que la preciosa gente de [Adobe](http://www.adobe.com/) [tarda siglos en portar a 64 bit](http://blogs.adobe.com/penguin.swf/2006/10/whats_so_difficult_64bit_editi.html) s su [plugin de flash](http://www.adobe.com/go/fp9_update_b1_installer_linuxplugin) (y no, probé [nspluginwrapper](http://www.gibix.net/projects/nspluginwrapper/) y me cuelga el Firefox). Me di cuenta que:

\- El plugin ["TabMixPlus"](https://addons.mozilla.org/firefox/1122/) no funca en firefox 2. Un garrón, aunque encontré un [post en este blog](http://www.luisbeltran.com/archivos/2006/09/firefox-2-beta-2-bien/) diciendo que están en proceso de sacar una actualización (recordemos que Firefox 2 hizo profundos cambios en el manejo de pestañas). Quise entrar a la [home page](http://tmp.garyr.net/) y veo que se quedaron sin ancho de banda disponible!! que alguien le tire una moneda a ese pibe, esa extensión es imprescindible!!

\- Quiero mi " [Ubuntu Human Theme](https://addons.mozilla.org/firefox/3008/)" para Firefox 2! (como tiene la versión de 64 bits). Claro, el paquete para integrar los íconos de Firefox 2 al skin de Ubuntu ahora viene incluído en los repositorios de Edgy... y no lo puedo instalar (lo busqué, eh!) como plugin .xpi para el Firefox de 32 bits. Por suerte, (así es el mundo Open Source, je) bajé el correspondiente archivo .deb, lo descomprimí y [acá les dejo](http://www.sharebigfile.com/file/14763/human-theme-ffox2-tar.bz2.html) mi archivo comprimido para que extraigan en el directorio de 'extensions' de firefox 2 (calculo que sólo sirve para Ubuntu Edgy).

\- Para volver el ícono del Thunderbird a la versión original, el script de [este thread](http://ubuntuforums.org/showthread.php?t=199193) anduvo, pero ejecutándolo como "sudo bash /usr/local/bin/restore\_mozilla\_icons".

Gracias a Beryl y los parches para que la transparencia de gnome-terminal sea "real" (sólo con X.org 7.1 y window managers que soporten Composite), tengo consola con fondo transparente de verdad!! :-D

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/Pantallazo.0.png)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/Pantallazo.0.png)

Otro problema (bastante importante) fue que las fuentes en Firefox, Gnome-terminal y OpenOffice se veían HORRIBLES. Encontré [este](https://launchpad.net/distros/ubuntu/+source/fontconfig/+bug/63403) thread, que me hizo reemplazar el archivo .fonts.conf de mi home por este:

```

rgb

true

hintfull

true

```

Y santo remedio! :-D
Evidentemente, hay problemas con Firefox y su manejo de fonts en linux... :-S

Otro "bugcito": tengo el problema (según [este](http://ubuntuforums.org/showthread.php?t=286334&highlight=usplash+grey) post somos varios) que nos muestra el splash de arranque en gris (?)

Por ahora, nada más. A medida que vaya encontrando problemitas, voy a ir publicando cómo los fui resolviendo. Por ahora, el balance general es bueno. El sistema vuela, es bastante estable (aún con XGL+Beryl instalado), y sí, es de saber que la actualización desde los repositorios tiende a tener este tipo de "glitches"... pero nada grave en mi opinión.

Las nuevas versiones de las aplicaciones, el arranque un poco más rápido (aunque todavía no creo que se le esté sacando el jugo a este esquema, es muy nuevo), etc. hacen que la actualización valga la pena largamente.

Saludos!
Marcelo

Actualización: También soy víctima [de este pequeño](https://launchpad.net/distros/ubuntu/+source/ubuntulooks/+bug/29508) "bugcicicicito":
[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/Pantallazo-Moviendo%20archivos.png)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/Pantallazo-Moviendo%20archivos.png)
Demasiado visible como para que saquen Edgy con este bug, no? :-S
En mi opinión esto (y comentarios que ví en varios lados) remarca que Edgy no es una distro LTS; o sea, es un poco más 'experimental' y para gente que quiere algo con algunos bugs pero con software más actualizado.
