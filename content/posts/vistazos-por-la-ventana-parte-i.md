---
author: Marcelo
category:
  - codear
  - tests
date: "2006-09-10T01:16:00+00:00"
guid: https://blog.marcelofernandez.info/?p=12
title: Vistazos por la Ventana - Parte I
url: /2006/09/vistazos-por-la-ventana-parte-i/

---
Hellou.... estoy testeando [Windows Vista](http://www.microsoft.com/windowsvista/default.aspx) RC1 5600.... les cuento con que me encontre hasta ahora. (Atención: Lo que Ud. va a leer es un pequeño resumen de la impresión que me causó usarlo por primera vez, y considere que utilizo y defiendo [GNU/Linux](http://es.wikipedia.org/wiki/Linux) full time. Sin embargo, voy a tratar de ser justo e imparcial; puede que me olvide de algo o se me escape algún detalle).

\- Para poder instalarlo es necesario aprox. 8GB de disco (muuuy aproximadamente :P )

\- Tuve el siguiente mensaje en la parte de particionamiento de la instalación: "Unable to find a system volume that meets its criteria for installation". Busqué un poco en el gran oráculo (el de los anteojos) y me [dijo](http://help.lockergnome.com/vista/Windows-unable-find-system-volumeftopic-7572-days0-orderasc-40.html) que tenía que setear la partición donde iba a instalar Vista como activa (con alguna herramienta como el [GParted](http://gparted.sourceforge.net/), por ejemplo). Lo hice y no anduvo, tuve de nuevo ese mensaje super-explicativo.... :-P

Como para probar como última vez, tuve que "sacar" mi disco IDE (donde tengo instalado el [OSX](http://www.apple.com/macosx/tiger/)), dejando conectado el disco SATA. No lo saqué físicamente, le puse "None" en el BIOS. Vista me lo seguía mostrando a pesar de haberlo desactivado en el BIOS (al parecer descubre los dispositivos como Linux, no pasando por la BIOS), pero esta vez no tuve el bendito error, el proceso de instalación continuó.

\- El resto fue bastante sencillo. Me detectó todo menos el sonido (una [SB](http://us.creative.com/products/product.asp?category=1&subcategory=206&product=10315) 5.1). Apenas arrancó, tuve que configurar la red. En mi opinión, tarde mucho en encontrar el cuadro de diálogo para poner la IP/Default Gateway/DNS de la placa de red ethernet. Como que hay muchos asistentes y formas de configurar la red... quizás todavía no me acostumbro a la nueva organización del Control Panel.

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/shot2.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/shot2.jpg)

Me meto en Network and Internet -> Network and Sharing Center:

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/shot3.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/shot3.jpg)

Hice click un poco acá y allá... pero no encontré cómo setearle la IP con esta nueva interfaz. Por suerte para los viejitos como yo está la "vista clásica". :-D

\- Después de eso, Windows Update se ejecutó solito y me instaló unos drivers... entre ellos, el de mi placa de sonido. Sin haber reiniciado, los parlantes ya hacían ruido.

\- Probé el IE7 y ví cómo alguno que otro sitio que construimos con los chicos estaba roto. También vi el efecto de [flicker](http://en.wikipedia.org/wiki/Flicker_%28screen%29), con un menú en Javascript, que se eliminó al instalar los [drivers de Nvidia para Vista](http://www.nvidia.com/content/drivers/drivers.asp).

 [![](http://photos1.blogger.com/blogger2/448/981953459584652/320/shot4.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/shot4.jpg)  


\- Instalé la [JRE](http://java.sun.com). Al cerrar el item respectivo en el panel de control, veo este cuadro de diálogo:

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/shot1.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/shot1.jpg)

Y esto?? Mucho no me gusta, me parece excesivo... entre esto, y la pregunta a cada cosa que quiero ejecutar que me pide permiso ( [UAC - User Account Control](http://en.wikipedia.org/wiki/User_Account_Control))... uno siempre le termina dando "Aceptar"... como que se pierde la utilidad de la "seguridad mejorada" de Vista.

\- Las tildes en la configuración en español están al lado de la "p" en vez de al lado de la "ñ", como en XP.

\- Muy buena la Sidebar y los gadgets. Ignoro si se escriben en algún lenguaje tipo AppleScript o Python ("fácil", a eso me refiero).

\- La única aplicación para editar una foto es el Paint. Y lo peor es que no cambió nada! Si pego una imagen menor al tamaño de la imagen "blanca" que está por defecto cuando uno abre el programa, lo blanco queda... es decir, uno no puede elegir "Paste as new Image" siquiera... ( [Gimp](http://www.gimp.org), te extrañooooo....)

\- Visualmente, está muuuuy bueno... aunque le falta algo más de "vida", cosa que sí tiene [Compiz](http://en.wikipedia.org/wiki/Compiz).... este desktop, como un todo, tiene mejor integración (quizás por el hecho que Compiz todavía está en un profundo e intensivo desarrollo).

Bueh, en resumen, más allá de los cambios visuales... en un primer approach no hay nada nuevo-nuevo... es decir, las aplicaciones son casi las mismas, no hay ninguna funcionalidad "revolucionaria". Como en los últimos años, la computación está avanzando de a poco en el desktop, y esta versión (RC1, guarda) de Windows Vista parece reflejar esta tendencia. "Polish and Logical Evolution" lo titularía.

(Bah... es pura facha!!) :-D

Después les sigo contando.

Salutes  
Marcelo
