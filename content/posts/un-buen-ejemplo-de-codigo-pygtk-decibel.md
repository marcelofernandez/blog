---
author: Marcelo
category:
  - codear
  - linux
  - python
  - ubuntu-ar
date: "2007-10-14T15:05:00+00:00"
guid: https://blog.marcelofernandez.info/?p=87
title: 'Un buen ejemplo de código PyGTK: Decibel'
url: /2007/10/un-buen-ejemplo-de-codigo-pygtk-decibel/

---
[Decibel Audio Player](http://decibel.silent-blade.org/index.php?n=Main.HomePage) es un programa liviano, sencillo, no orientado a tener miles de funciones ( [Amarok](http://amarok.kde.org/), les suena? :-D ), sino sólo a reproducir música de una lista. La verdad que las [capturas de pantalla](http://decibel.silent-blade.org/index.php?n=Main.Screenshots) ("everybody loves screenshots!") pintaban prometedoras, y cuando leí que estaba hecho en Python + GTK, bajé el fuente a ver qué onda\[1\]...

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/RxIyXn7y13I/AAAAAAAAAL4/OcdQYNW4qxw/s400/main-file-explorer.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/RxIyXn7y13I/AAAAAAAAAL4/OcdQYNW4qxw/s1600-h/main-file-explorer.png)

Y la verdad que me está gustando lo que estoy leyendo. La parte de módulos me parece bárbara, da gusto leerla: para hacer un módulo propio (aka 'plugin'), uno hereda de una clase "Modulo" (también hay un ThreadedModule) y se registra a los eventos que quiere recibir, puede emitir eventos a los demás, etc. Las pantallas están hechas con [Glade](http://glade.gnome.org/), el código está bien comentado (y bien identado, es Python! :-D ), el tamaño del proyecto y del fuente es manejable (al usar Glade, casi no se ve código de manejo de ventanas), usa semáforos, locks, está bien modularizado y orientado a objetos, etc.

Vista de un .glade abierto (con el [vim](http://vim.sourceforge.net/) de fondo):

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/RxIz-n7y14I/AAAAAAAAAMA/-h7JTrukkQc/s400/Pantallazo.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/RxIz-n7y14I/AAAAAAAAAMA/-h7JTrukkQc/s1600-h/Pantallazo.png)  


Es resumen, si estás aprendiendo python (o programación, por qué no), leer el código fuente de este programa puede ser muy entretenido, para aprender cómo hacen las cosas los demás (y cómo se hacen las cosas bien hechas, je). Demás está decir que esto se puede hacer gracias a que es Software Libre, que si no... (es más, me gustaría meter algunas features que no me gustan y mandarlas al desarrollador...).

Espero que le pique el "bichito pythónico" a más de uno.

Saludos  
Marcelo  
\[1\]: Mentiira! :-D Como todavía uso Feisty (Decibel [ya está en los repos de Gutsy](http://packages.ubuntu.com/gutsy/sound/decibel-audio-player)), bajé [el .deb de la página](http://decibel.silent-blade.org/uploads/Main/decibel-audio-player-0.06.3.deb) y cuando GDebi me dijo que era para la arquitectura i386 (tengo un x86-64), putié y dije "por qué, si es python, viejo! tiene que ser multiplataforma!". Ahí bajé el código.  
PD: Guarda: Miren los \_\_init\_\_.py de los subdirectorios, ahí está siempre lo que uno busca. :-)

Actualización: Envié [un pequeño parche](http://launchpadlibrarian.net/10021323/decibel-0.06-hide_window_and_start_in_tray_patch.tar.gz), que implementa el ["cerrar ventana y no salir"](https://bugs.launchpad.net/decibel-audio-player/+bug/152698) y el ["iniciar minimizado"](https://bugs.launchpad.net/decibel-audio-player/+bug/146668), aunque necesita que el módulo cargue siempre el StatusIcon (no estaba así por defecto). Espero que el desarrollador principal lo acepte. :-)
