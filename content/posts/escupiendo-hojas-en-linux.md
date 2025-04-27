---
author: mfernandez
category:
  - codear
  - linux
  - programación
date: "2006-09-17T18:29:00+00:00"
guid: https://blog.marcelofernandez.info/?p=13
title: Escupiendo hojas en Linux
url: /2006/09/escupiendo-hojas-en-linux/

---
Holaaa!

En estos últimos días de silencio bloguero me enganché en unos proyectos de desarrollo en [Python](http://www.python.org/) (primero y por ahora) y el segundo (todavía no lo ví) en [PHP](http://www.php.net/).

Lo primero que tuve que resolver fue hacer que una impresora [Epson C67](http://www.epson.com.ar/asp/muestraProducto.asp?idProducto=C11C616031) imprimiese lo más rápido posible, y en modo gráfico (como para ponerlo en modo draft, mínimo). La gente había investigado dos opciones, una era abriendo el puerto /dev/usblp0 directamente, tratando de enviar la inicialización... miraron (y miré yo también) bastante código de CUPS, de sus filtros (en C)... tratando de entender el lenguaje EJL, cambiarlo a [IEEE 1284.4](http://en.wikipedia.org/wiki/IEEE_1284) y después a ESCP/2... fue imposible. Hay muy poca documentación y lo máximo que se logró fue que tomara la hoja, pero nada más. Si alguien quiere documentación, [acá](http://undocprint.printassociates.com/formats/printer_control_languages/ejl) tiene algo más.

Como alternativa se podía imprimir por [CUPS](http://www.cups.org/). Pero CUPS es un soft grandote, que como se puede ver en [su arquitectura](http://www.cups.org/documentation.php/spec-design.html), es cliente/servidor, maneja un spooler... filtros, planificación, ACLs, es decir, consume mucho procesamiento antes de que efectivamente imprimiera... (tomaba 10 segundos hasta que tomara la hoja, lo cual para los objetivos del proyecto era inaceptable).

Esto está en una parte de la [extensa documentación](http://www.cups.org/documentation.php/overview.html):
[![](http://www.cups.org/images/cups-block-diagram.gif)](http://www.cups.org/images/cups-block-diagram.gif)
Hasta que leí con más detenimiento la página de [Linux Printing](http://linuxprinting.org/). Allí [dice](http://linuxprinting.org/foomatic.html) que los drivers Foomatic (con los que se imprime en la C67) pueden utilizar como output CUPS, LPR, PPR, PDQ, Y [SIN SPOOLER](http://linuxprinting.org/direct-doc.html)!! (Salvo CUPS, al resto de los spoolers no los conozco). O sea, impresión directa con los drivers de foomatic.

Era casi ideal la situación, salvo que había que ir a la práctica. Siguiendo la documentación salió todo perfecto. En una máquina actual (Celeron 2 Ghz), desde que se ordena la impresión hasta que toma la hoja y comienza a imprimir tardó 1.8 segundos! (en modo draft, enviándole un pdf como input). Cabe destacar que la generación del pdf sencillo tomaba menos de un segundo.

Más o menos fue tanto como copiar el archivo .ppd correspondiente a /etc/foomatic/direct/ y después ejecutar:

```
#foomatic-rip --ppd /etc/foomatic/direct/impresora.ppd \
/home/marcelo/test
```

(todo en la misma línea)

Desde python fue tanto como ejecutar esto mismo con una llamada a os.system().

Así solucioné el primero de los dos problemas que tuve esta semana. El segundo lo dejo para la próxima, porque todavía no lo pude resolver del todo, pero tengo buenas perspectivas....

Me voy a disfrutar un poco del sol y de la familia.

Salutes!
Marcelo

PD: Me reí un rato largo con todas las fotos (y la imaginación de los que juegan con Photoshop o [Gimp](http://www.gimp.org)) del ["Tio del Cigarro"](http://zonaforo.meristation.com/foros/viewtopic.php?t=354619) Mírenlo! :-D
