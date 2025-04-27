---
author: mfernandez
category:
  - codear
  - linux
date: "2007-01-03T13:32:00+00:00"
guid: https://blog.marcelofernandez.info/?p=39
title: SysRQ remoto
url: /2007/01/sysrq-remoto/

---
[![](http://upload.wikimedia.org/wikipedia/commons/6/6c/My_Opera_Server_Back.jpg)](http://upload.wikimedia.org/wikipedia/commons/6/6c/My_Opera_Server_Back.jpg)  
Si alguien conocía las " [teclas mágicas](http://aplawrence.com/Words2005/2005_04_13.html)" de GNU/Linux, sabía que sólo funcionaban localmente, es decir, estando uno delante del Server. Sin embargo, si uno está lejos físicamente, se complica... (sí, un amigo llamaba a la casa para que la madre le reiniciara el Servidorcito... :-P ).

Para eliminar este inconveniente que han desarrollado [sysrqd](http://julien.danjou.info/sysrqd.html), un software que funciona como demonio sólo para atender a "system requirements", o sea, poder enviar remotamente esas "teclas mágicas". Está pensado para consumir lo menos posible de CPU (calculo que es por eso que no encripta el tráfico), no inicializa ningún shell y sólo tiene autenticación del tipo usuario/contraseña.

Después de identificarse, se presiona la combinación de teclas que se desea y listo. :-D

Algunos links más:  
[Debian Package of the Day](http://debaday.debian.net/2007/01/03/sysrqd-small-daemon-to-manage-linux-sysrq-over-network/)  
[Raising Skinny Elephants Is Utterly Boring](http://es.wikipedia.org/wiki/Raising_Skinny_Elephants_Is_Utterly_Boring)

Saludos!  
Marcelo  
PD: Gracias [Wikipedia](http://www.wikipedia.org) por la imagen.
