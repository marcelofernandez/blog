---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2007-07-15T18:07:00+00:00"
guid: https://blog.marcelofernandez.info/?p=76
title: 'MPX: Multi-Point X Server'
url: /2007/07/mpx-multi-point-x-server/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/RpwuiB9dfoI/AAAAAAAAAJQ/xskDwhvu9WA/s320/multitouch.jpeg)](http://4.bp.blogspot.com/_nDZ247g0qSM/RpwuiB9dfoI/AAAAAAAAAJQ/xskDwhvu9WA/s1600-h/multitouch.jpeg) Buenas...

Hace unos meses había salido la noticia de la presentación de [Microsoft Surface](http://www.microsoft.com/surface/), que aunque no va a estar disponible hasta fines de 2007, sus videos permiten apreciar una nueva manera de interactuar con una computadora, ya que el [Touch Screen](http://en.wikipedia.org/wiki/Touch_screen) horizontal posee un sensor [Multi-touch](http://en.wikipedia.org/wiki/Multi-touch). Una combinación de hardware y software donde una o varias personas interactuaban en una "mesa digital", al mismo tiempo... realmente es muy interesante! (más alla de que Microsoft lo sepa vender bien).

Como [decía Angel Lopez](http://msmvps.com/blogs/lopez/archive/2007/06/12/microsoft-surface.aspx) en su momento, "para la adopción de esta tecnología, es fundamental que exista software que lo aproveche y que su costo sea accesible". Creo que teniendo en cuenta lo segundo, será difícil la implementación de Surface dado el contexto y la situación argentina, conociendo la estrategia de "precios globales" (léase: globales = en dólares accesibles para la gente del primer mundo) de Microsoft (aunque cada tanto [estas noticias aisladas](http://www.pergaminovirtual.com.ar/revista/cgi-bin/hoy/archivos/00000692.shtml) contradigan esta política y parezca que ceden ante la realidad del tercer mundo, sólo es la confirmación de que portándose así pierden competitividad frente al software libre/abierto).

Pero bueh, no me quiero desviar del tema. :-D

Pensándolo como implementación alternativa a esta tecnología, me encontré en algún post de algún blog, la página del [Multi-Point X Server](http://wearables.unisa.edu.au/mpx/?q=mpx), que como dice su página es una modificación al [X Server](http://es.wikipedia.org/wiki/X_Window_System) tradicional, que permite que varios dispositivos de entrada interactúen con las aplicaciones que todos conocemos ("legacy") más el desarrollo de nuevas aplicaciones, dando soporte para los n dispositivos de entrada y n focos en la misma pantalla.  

[![](http://3.bp.blogspot.com/_nDZ247g0qSM/Rpws0x9dfnI/AAAAAAAAAJI/mmE-ljHyqLo/s400/MPX+The+Multi-Pointer+X+Server.png)](http://3.bp.blogspot.com/_nDZ247g0qSM/Rpws0x9dfnI/AAAAAAAAAJI/mmE-ljHyqLo/s1600-h/MPX+The+Multi-Pointer+X+Server.png)

Ahora, si combinamos el Multi-Pointer X Server, con una tableta [Diamond Touch](http://www.merl.com/projects/DiamondTouch/) (que soporta el multitouch), obtenemos esto:  

[Click here to block this object with Adblock Plus](http://www.youtube.com/v/olWjnfBoY8E "Click here to block this object with Adblock Plus")[Click here to block this object with Adblock Plus](http://www.youtube.com/v/olWjnfBoY8E "Click here to block this object with Adblock Plus")[Click here to block this object with Adblock Plus](http://www.youtube.com/v/olWjnfBoY8E "Click here to block this object with Adblock Plus")Como pueden ver, este bicho corre Ubuntu (o Kubuntu, no se ve). :-P

Pero sí se aprecia el Google Earth, Firefox, el Gimp y hasta un programa (tipo Paint) seguramente programado según la [interfaz extendida al XInput](http://wearables.unisa.edu.au/mpx/?q=developingapps) (aunque lo importante es que es compatible hacia atrás). Al final del [artículo con el video](http://wearables.unisa.edu.au/mpx/?q=node/86), se aclara que esta versión de XServer es independiente del dispositivo hardware; "usted puede utilizar su DiamondTouch, su [tabla FTIR](http://tinker.it/now/2007/02/28/multitouch-table-experiment/), o - si usted puede comprar una - su tabla MS Surface".

Y la remata con:  
"Ah, y por si acaso; usted puede utilizar un mouse estándar y un teclado en la misma CPU al mismo tiempo que utiliza el touchscreen. Después de todo, cualquier cosa es sólo un dispositivo más. Este es el último gran cambio, luego voy a subir los cambios al proyecto MPX".

Luego, [en este post](http://wearables.unisa.edu.au/mpx/?q=node/87), luego de la avalancha de visitas por el video de Youtube, se brindan más detalles.

[Hay paquetes para Ubuntu Feisty](http://wearables.unisa.edu.au/mpx/?q=node/85). Alguien se anima a probarlo con dos (o más) mouses y dos (o más) teclados? :-D

Saludos!  
Marcelo
