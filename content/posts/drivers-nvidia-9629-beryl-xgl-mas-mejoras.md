---
author: Marcelo
category:
  - codear
  - linux
  - tests
  - ubuntu-ar
date: "2006-11-09T01:44:00+00:00"
guid: https://blog.marcelofernandez.info/?p=22
title: Drivers Nvidia 9629 + Beryl - XGL = Más mejoras
url: /2006/11/drivers-nvidia-9629-beryl-xgl-mas-mejoras/

---
Hola gente, por fin salieron nuevos [drivers de Nvidia](http://www.nvidia.com/object/linux_display_amd64_1.0-9629.html) para Linux/BSD/Solaris!!! (en su versión estable, claro).

Son los primeros de la serie 9xxx, los cuales permiten que en mi [Edgy](http://www.ubuntu.com/) vuele de un plumazo a [XGL](http://en.wikipedia.org/wiki/Xgl) con el fin de ejecutar [Beryl](http://www.beryl-project.org/)... Ahora sólo ejecuto [X.org 7.1.1](http://x.org/) \+ Beryl.

Es decir, antes: X.org + XGL + Beryl. (Drivers Nvidia 8xxx)

Problema Principal: OpenGL, XVideo y otras extensiones que renderizaban directamente en la memoria de video (por decirlo sencillo, alguien que aclare acá!) no era directo, sí o sí pasaba por el server XGL.

Consecuencias: Lentitud en cualquier cosa que utilice esas operaciones, como ser juegos o reproductores de video. El smooth scrolling en firefox (ignoro por qué era lento antes).

Ventajas: Efectos 3D en el escritorio! Cubos girando, zoom, [exposé](http://en.wikipedia.org/wiki/Expos%C3%A9_%28Mac_OS_X%29) y varias cosas bonitas.

Ahora: X.org 7 + Beryl. (Drivers Nvidia 9xxx). Al estar estos drivers preparados para soportar las operaciones de un [window manager](http://en.wikipedia.org/wiki/Window_manager) como Beryl directamente, no hace falta XGL, por lo tanto no hace falta un servidor X (XGL) que haga llamadas OpenGL a otro (X.org). Directamente Beryl hace llamadas a X.org.

Menos Componentes = Más velocidad. Y con más facilidades, como por ejemplo OpenGL renderizando con aceleración directa! (sin trucos, Doom 3 y Quake 4!)

Acá hay una captura de un momento glorioso: OpenGL + Composite + Beryl.

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/Pantallazo.1.png)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/Pantallazo.1.png)

Sí, se comía la CPU (como pueden ver en el sensor de carga de CPU), pero era el glxgears, ya que probé el [Tux Racer](http://en.wikipedia.org/wiki/Tux_Racer) y el [Super Tux](http://www.happypenguin.org/show?SuperTux) (juegos que usan OpenGL) y apenas la CPU llegaba al 40% en los dos casos, corriendo [Metacity](http://en.wikipedia.org/wiki/Metacity) y Beryl.

Instalación  
Desde cero es algo largo, pero partiendo de una instalación de XGL + Beryl es relativamente simple (aunque depende de qué howto hayan seguido). Básicamente, fue:

1 - Bajar los nuevos drivers de NVidia. Usé [estos repositorios](http://albertomilone.com/driver_edgy.html), que preguntando al mantenedor me dijo que "me quede tranquilo", que cuando Ubuntu actualice el kernel él iba a actualizar los paquetes. :-D

2 - Probar que todo funcionara como antes (por las dudas). O sea, si quería podía seguir usando XGL.

3 - Cambiar la configuración de [GDM](http://en.wikipedia.org/wiki/GNOME_Display_Manager) para que no arranque el XGL, sino que cargue X.org. Sólo fue modificar el /etc/gdm/gdm.conf-custom (comentando las 3 líneas que había puesto, bah). Lo dejé como cuando vino del CD de instalación de Ubuntu. ;-D

4 - Agregar una opción al /etc/X11/xorg.conf (como se explica [acá](http://forum.beryl-project.org/post-41432#p41432)).

5 - Listo. Arrancar la sesión de mi usuario y probar...

Bueh, es todo lo que me acuerdo; estuve jugando bastante y el sistema está más liviando; aunque me gustaría que en la nueva versión de Beryl (0.1.2, yo todavía estoy corriendo 0.1.1), se corrijan varios errores muy visibles, y se optimice el funcionamiento en general (tiene algunos cuellos de botella todavía).

Taluego  
Marcelo
