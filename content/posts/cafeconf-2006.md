---
author: Marcelo
category:
  - codear
  - linux
  - misceláneos
  - personal
date: "2006-11-15T03:47:00+00:00"
guid: https://blog.marcelofernandez.info/?p=23
title: Cafeconf 2006
url: /2006/11/cafeconf-2006/

---
Al fin llegó la [Cafeconf 2006](http://www.cafeconf.org/modules/edito/content.php?id=1)! :-D

Estuvo buena, lástima que este año pude ir sólo un día.

Lo destacable fue encontrarse con toda esa "gente como uno" que me hace sentir que no estoy "solo" en el mundo. :-D

Sobre las charlas a las que asistí, les puedo comentar lo siguiente:  

- " [Collition Course with Ruby](http://www.gabriel-arellano.com.ar/charlas/)": Excelente charla, muuuy entretenida e interesante; el disertante ( [Gabriel Arellano](http://www.gabriel-arellano.com.ar/)), un capo, muy didáctico y llevadero, no aburrió para nada. Aprendí muchas cosas de Ruby. Lástima que el tiempo (como en la TV) es tirano.

- " [Cómo Hacer Plata con Software Libre](http://www.cafeconf.org/modules/myconference/viewspeech.php?sid=114&cid=1)": Ummm... odio reconocerlo, pero en mi opinión, estuvo muy floja. Con este título esperaba (y creo que todos los asistentes esperábamos) mucho más. Le debo mucho respeto y reconocimiento a [John Lenthon](http://www.cafeconf.org/modules/myconference/viewcv.php?cvid=69), lo conozco de la lista de PyAr y es un capo, sabe muchísimo, pero en mi humilde opinión (tratando de ser constructivo) podría haber planificado de otra manera un tema tan interesante.

- " [XiFrame, un poderoso framework de desarrollo de aplicaciones](http://www.cafeconf.org/modules/myconference/viewspeech.php?sid=79&cid=1)": Muy buena, les debo el comentario.

- Keynote - " [Que es Python y Porqué importa](http://www.cafeconf.org/modules/myconference/viewspeech.php?sid=27&cid=1)", de [Alex Martelli](http://en.wikipedia.org/wiki/Alex_Martelli): Excelentísimo disertante, clarísimo en lo que quería transmitir; lástima que no hubo otra charla con contenidos más avanzados. Me dejó el concepto del "espíritu" del lenguaje, fue un honor escucharlo.

- " [Routing avanzado y balanceo de carga en GNU/Linux](http://www.cafeconf.org/modules/myconference/viewspeech.php?sid=73&cid=1)": MUY buena, más que nada porque aunque hice cosas con firewalls, proxys, servers Linux, nunca tuve la oportunidad de investigar un poco sobre balanceo con varias conexiones de banda ancha. La respuesta es sencilla: "apt-get install iproute", "man iproute". O si no, googleen. :-D

- " [Voto Electrónico](http://www.cafeconf.org/modules/myconference/viewspeech.php?sid=131&cid=1)" \- [Enrique Chaparro](http://www.cafeconf.org/modules/myconference/viewcv.php?cvid=95&cid=1): Les debo el comentario. Estuvo bien, con algunas salvedades que después les comentaré (es tarde y hay que hacer noni). :-D

- Después no me quedé al sorteo y cánticos alegóricos... pero tienen que saber que los acompaño en el sentimiento y participo de sus foros, logros y eventos. Salud, Cafelug! (con versito y todo!)

Una de las cosas que quiero plasmar acá es haber escuchado de Sebastián Desimone una idea bastante piola, como un Proxy de SQLs (de un SGBD, en realidad). De acá en más, volé... y se me ocurrió algo como lo siguiente, algo como:

"Concepto de un "Firewall de SQLs", utilizando un "Proxy de BD" (que implementa el protocolo de servidor de una SGBD), pero que 'filtra' qué consultas están permitidas y qué consultas no, evitando ataques del tipo de Inyección SQL."

Además de eso, se me ocurre que uno puede distribuir con una aplicación web (Joomla, por ejemplo) un "modulito" con un conjunto de consultas SQL que utiliza la aplicación, que se instala en el Firewall de SQLs. Luego, tu aplicación está proactivamente protegida (un poco más) contra errores de programación (del Joomla, en este hipotético caso).

Es más, hasta se puede poner el firewall en modo "promiscuo" (es decir, que deje pasar todas las consultas), y que todo lo que redirecciona a la BD lo vaya almacenando aparte como log. En ese momento, se utiliza la aplicación web normalmente, abarcando toda su funcionalidad. Luego, se sale del modo "promiscuo" del Firewall de SQL y se utiliza el log con las consultas hechas anteriormente como base de reglas de filtrado, haciendo que la aplicación pueda realizar sus operaciones, pero si surge algún problema de seguridad en forma de inyección SQL, dicho ataque no pase por el "firewall".

Calculo que algo de esto debe existir (BDs grosas, como Oracle, DB2, etc.), pero es la primera vez que tengo noción de algo así, y tán útil!!!

Ahora sí, volviendo al planeta tierra, paso a dejarles las pocas fotos que saqué del tan esperado evento...

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/IMG_0807.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/IMG_0807.jpg)  
Con Rocío, Pablo y Luis en la puerta.

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/IMG_0812.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/IMG_0812.jpg)  
Los muchachos del UnluX, siempre presentes!

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/IMG_0808.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/IMG_0808.jpg)  
Mauro y Efra, próximos pythonistas. :-D

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/IMG_0811.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/IMG_0811.jpg)  
"Alex Martelli in Concert" :-D

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/IMG_0810.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/IMG_0810.jpg)  
Devorando en Mc Pato.

[![](http://photos1.blogger.com/blogger2/448/981953459584652/320/IMG_0809.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/IMG_0809.jpg)  
Otra foto, esta vez con Ezequiel agregado al grupete, lástima que el muchacho le dió rápido al botón de la cámara y salió fuera de foto. :-(

Bueh, es todo, voy a dormiiiir.  
Salutes  
Marcelo
