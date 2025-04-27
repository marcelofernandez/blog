---
author: mfernandez
category:
  - codear
  - linux
  - python
date: "2007-08-14T13:23:00+00:00"
guid: https://blog.marcelofernandez.info/?p=81
title: Youtube usa Python
url: /2007/08/youtube-usa-python/

---
[![](http://2.bp.blogspot.com/_nDZ247g0qSM/RsGsm09PgGI/AAAAAAAAAJ4/7k6yg0v_PqA/s400/pic_youtubelogo_123x63.gif)](http://2.bp.blogspot.com/_nDZ247g0qSM/RsGsm09PgGI/AAAAAAAAAJ4/7k6yg0v_PqA/s1600-h/pic_youtubelogo_123x63.gif)  
[En esta página](http://highscalability.com/youtube-architecture) hay un poquito de detalle sobre la arquitectura que utiliza YouTube, que sirve alrededor de 100.000.000 (sí, cien millones) de videos por día.

Adivinen qué? Usa Linux + un Application Server escrito en Python. :-D

[Psyco](http://psyco.sourceforge.net/) es un "acelerador" de python, digamos que compila "al vuelo" el código que interpreta python y lo convierte a código nativo de la plataforma (como si se hubiera escrito en C, digamos). Si a ellos les da resultados, se podría probar para casos donde se necesite performance.

También me sorprende que usen [lighthttpd](http://www.lighttpd.net/), que es un webserver "liviano" (traducido: "más liviano que Apache"); en ese caso, quiere decir que está listo para utilizar en producción. :-)

Para más detalles lean la página, está muy interesante.

Saludos  
Marcelo
