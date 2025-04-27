---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2007-07-14T20:58:00+00:00"
guid: https://blog.marcelofernandez.info/?p=75
title: Samsung Syncmaster 206/226 NW en Ubuntu
url: /2007/07/samsung-syncmaster-206226-nw-en-ubuntu/

---
Tengo chiche nuevo!

Después de algún tiempo de sacrificado laburo, me dí uno de los mayores gustos "informáticos" hasta ahora: comprarme un monitor LCD de 22" wide... más precisamente el [Samsung Syncmaster 226 NW](http://www.samsung.com/ar/products/monitor/lcd2040/226nw.asp). :-D

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/Rpk6xR9dfhI/AAAAAAAAAII/7nxBv1XKBbs/s400/IMG_2058.JPG)](http://2.bp.blogspot.com/_nDZ247g0qSM/Rpk6xR9dfhI/AAAAAAAAAII/7nxBv1XKBbs/s1600-h/IMG_2058.JPG)

De más está decir que es fantabuloso cómo se ve cualquier cosa que uno quiera ver. No hay "efecto arrastre" al hacer scroll, excelente contraste, excelente resolución y nitidez... Lo único "malo" técnicamente es que no viene con conexión [DVI](http://es.wikipedia.org/wiki/Digital_Visual_Interface).

Sobre esto último, leí por ahí que en Argentina el IVA es del 21% para los monitores con [DVI](http://es.wikipedia.org/wiki/Digital_Visual_Interface), en cambio para los con conexión [VGA analógica](http://es.wikipedia.org/wiki/VGA) es el 10,5%; de hecho, acá en Argentina los 206/226 BW (son iguales pero con DVI) no se venden (un garrón).

Llegué a casa, saqué el viejo y querido [Samsung 796MB](http://www.samsung.com/ar/products/monitor/flatcdt/796mbplus.asp) y me dispuse a disfrutar mi Ubuntu Feisty con él. Claro, Ubuntu estaba configurado con la "vieja" resolución (1024x768). Tenía que cambiar a la "nueva" resolución: 1680x1050 (los monitores LCD siempre deben estar a resolución nativa); para eso, abri una consola y ejecuté "dpkg-reconfigure xserver-xorg". Joya, seleccioné las resoluciones que quería (1680x1050 entre ellas), me generó un xorg.conf nuevo (con las resoluciones/configuraciones seleccionadas, haciendo un backup del xorg.conf anterior) y reinicié la PC.

Pero me encontré con un (bug?) problema: [Gdm](http://en.wikipedia.org/wiki/GNOME_Display_Manager) no arrancaba! :-(

El síntoma era: [X](http://es.wikipedia.org/wiki/X.Org) arrancaba, parece que arrancaba de nuevo, veía el fondo "cuadriculado" en blanco y negro típico de X y el cursor de Gnome en "espera". Listo, no pasaba más de ahí. Era cuestión de hacer Ctrl+Alt+F1 y loguearse en la consola de texto. Hice un "top" y me mostraba el Gdm funcionando, pero comiéndose la CPU al 100%! No quedaba otra más que matarlo. Arrancando con el Live-CD de Ubuntu, el proceso de arranque detectaba los 1680x1050 y pude usar Gnome desde el CD como si nada.

Si volvía al xorg.conf anterior, funcionaba, pero no tenía resolución nativa. Utilizando nvidia-settings claro que podía configurar el monitor a la resolución que quería, pero setearlo a mano cada vez que iniciaba sesión no me parecía una opción. Tenía que googlear, y encontré [este link](https://help.ubuntu.com/community/FixVideoResolutionHowto). Sencillito. Fui hasta la parte de "drivers nvidia" (tengo una Nvidia 6600) y ejecuté el nvidia-settings como root, generé con el programa un xorg.conf nuevo desde ahí y restartié gdm. Ahora sí arrancaba bien, y a 1680x1050.

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/RplA5B9dfiI/AAAAAAAAAIQ/1RPb7wGWx_s/s400/Pantallazo.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/RplA5B9dfiI/AAAAAAAAAIQ/1RPb7wGWx_s/s1600-h/Pantallazo.png)

Pero me faltaban los efectos de [Beryl](http://beryl-project.org/) ( [Compiz Fusion](http://www.opencompositing.org/) está en desarrollo todavía). Claro, el nvidia-settings sí incluye la opción de GLX, pero no incluye el Composite (creo que es necesario todavía)... bah, en realidad me generó un xorg.conf bastante limpito, y tuve que copiar/pegar algunas opciones de configuración que en su momento agregué para Beryl.

[Este es el xorg.conf que me quedó, para el que lo quiera bajar.](http://rapidshare.com/files/42940308/xorg.conf.html)

Conclusión:  
Bueh, fue un rato dando vueltas; a Ubuntu (y las distros GNU/Linux en general) les queda un poco de laburo el tema de la configuración de las resoluciones de los monitores (estamos en el 2007 y uno tiene que configurar el xorg.conf a mano...). Esperemos que en [Ubuntu 7.10 Gutsy Gibbon](https://launchpad.net/ubuntu/gutsy/), con [este proyecto que va a estar incluído](https://wiki.ubuntu.com/DisplayConfigGTK), más [X.org 7.3](https://blueprints.launchpad.net/ubuntu/+spec/xorg7.3), este problema se solucione.

(ahora, a disfrutar!)

Saludos!  
Marcelo
