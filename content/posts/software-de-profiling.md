---
author: Marcelo
category:
  - codear
  - programación
date: "2007-06-15T02:47:00+00:00"
guid: https://blog.marcelofernandez.info/?p=68
title: Software de Profiling
url: /2007/06/software-de-profiling/

---
[![](http://3.bp.blogspot.com/_nDZ247g0qSM/RnIFQac5XBI/AAAAAAAAAHA/XRvuM30mwPE/s320/101300834_945328f91e.jpg)](http://3.bp.blogspot.com/_nDZ247g0qSM/RnIFQac5XBI/AAAAAAAAAHA/XRvuM30mwPE/s1600-h/101300834_945328f91e.jpg) Buenas... leyendo [este post](http://www.kaourantin.net/2007/02/limits-of-software-rendering.html) sobre cómo se analizó el comportamiento a bajo nivel en el algoritmo de renderizado del [Flash Player](http://www.adobe.com/products/flash/about/) , me interesé sobre las herramientas que nombra y utiliza:  

- [Code Analyst](http://developer.amd.com/calinux.jsp) de AMD (recomendado para hacer Profiling sobre micros AMD).
- [VTune](http://www.intel.com/cd/software/products/asmo-na/eng/vtune/239144.htm) de Intel (ídem para Intel).  

En el caso del Code Analyst, está disponible para Linux y Windows (x86 y x86-64), y está disponible su código fuente (dice que es "open source", no encontré bajo qué licencia). Estuve chusmeando las [diapositivas estas](http://developer.amd.com/assets/Linux_Summit_PJD_2007_v2.pdf), la verdad que me sorprendió gratamente que exista este tipo de herramienta (digo, que parece ser de calidad, además de abierta y gratuita).

El soft de Intel no me interesó porque enseguida vi que es cerrado y sale [U$S 699](http://www.intel.com/cd/software/products/asmo-na/eng/download/locations/index.htm#vtune) (y además, para qué si la misma gente de Adobe ex-Macromedia usa el de AMD?). La competencia es buena para el consumidor...

Obviamente que existen muchísimas herramientas como éstas, para diversos lenguajes ( [Netbeans](http://www.netbeans.org/) [tiene para Java un profiler](http://profiler.netbeans.org/) [que es bellísimo](http://profiler.netbeans.org/screenshots.html)), pero considero que si uno está laburando con código en C, buscando el máximo de performance, el CodeAnalyst puede servir. Lo anoto en mis apuntes por si algún día lo necesito.

Sólo mis 2 ctvs.  
Marcelo  
PD: Imagen extraída de [esta URL.](http://flickr.com/photos/rob20/101300834/) Gracias [Rob](http://flickr.com/photos/rob20/)! (La imagen está bajo la [licencia CC](http://creativecommons.org/): Attrib-NoComm-ShareAlike)  
