---
author: mfernandez
category:
  - codear
  - linux
  - python
  - sysadmin
  - ubuntu-ar
date: "2008-10-11T16:25:00+00:00"
guid: https://blog.marcelofernandez.info/?p=110
title: Análisis de Performance de un Script Python
url: /2008/10/analisis-de-performance-de-un-script-python/

---
Hola!

De toda la pila de mails que recibo, leo y analizo (cuando puedo) de [PyAr](http://www.python.com.ar/moin/ListaDeCorreo), me interesó mucho uno de [este hilo](http://thread.gmane.org/gmane.org.user-groups.python.argentina/16293), referido al [lenguaje DOT](http://en.wikipedia.org/wiki/Dot_language) y su utilización práctica en el desarrollo en general. Básicamente, [se comentó](http://article.gmane.org/gmane.org.user-groups.python.argentina/16299) lo útil que es graficar la salida de la ejecución de un [profiler](http://en.wikipedia.org/wiki/Performance_analysis), con este generador de lenguaje DOT basado en el log de cualquiera de los 3 módulos de [profiling de Python](http://docs.python.org/library/profile.html) \[1\]\[2\]: [Gprof2Dot](http://code.google.com/p/jrfonseca/wiki/Gprof2Dot).

Brevemente, lo probé con un script que tengo para armar un archivo de texto con cierto formato, resultado de un query a una base de datos. Ejecuté esto, tal como dice el sitio de la herramienta:

```
$ python -m cProfile -o output.pstats ./mkmenu.py
$ gprof2dot.py -f pstats output.pstats | dot -Tpng -o output.png
```

Y obtuve este precioso gráfico:
[![](http://1.bp.blogspot.com/_nDZ247g0qSM/SPDmNgCs79I/AAAAAAAABPM/jjePLFTH9Rk/s320/output.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/SPDmNgCs79I/AAAAAAAABPM/jjePLFTH9Rk/s1600-h/output.png) El cual demuestra que una vieja regla sigue teniendo vigencia: "no importa lo que hagas en tu programa, el cuello de botella va a estar en la base de datos". :-)

Más allá de eso, me encantó esta manera gráfica y elegante de buscar "hot spots" donde uno puede ir y optimizar el código.

Saludos
Marcelo

\[1\]: Python tiene 3 módulos para hacer profiling, 2 relacionados ( [profile y cProfile)](http://docs.python.org/library/profile.html) y uno aparte ( [hotshot](http://docs.python.org/library/hotshot.html#module-hotshot)). Según el caso, uno es más adecuado que otro. Hotshot ya está en desuso y en próximas versiones se lo podría quitar de la librería estándar.
\[2\] Gprof2Dot también sirve para graficar el log de un log [oprofile](http://oprofile.sourceforge.net/) (es un profiler genérico para Linux).
