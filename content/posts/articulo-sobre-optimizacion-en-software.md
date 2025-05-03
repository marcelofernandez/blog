---
author: Marcelo
category:
  - codear
  - programación
  - python
  - ubuntu-ar
date: "2009-02-19T01:04:00+00:00"
guid: https://blog.marcelofernandez.info/?p=125
title: Artículo sobre Optimización en Software
url: /2009/02/articulo-sobre-optimizacion-en-software/

---
En [este artículo](http://www.shlomifish.org/philosophy/computers/optimizing-code-for-speed/) el autor ( [Shlomi Fish](http://www.shlomifish.org/)) discute un poco sobre optimización de algoritmos, y está orientado bastante a lo pragmático, desmitificando (o intentando derribar) algunos viejos "axiomas" de la programación.

Para mí fue muy interesante la introducción de la idea de que optimizar factores constantes en la complejidad de un algoritmo también sirve; es decir, no sólo reducir órdenes de magnitud como único objetivo al optimizar, sino que también es importante hacerlo en factores. Con lo cual tiende a derribar dentro de su criterio el mito de "O(N²) es igual a O(2N²)", por ejemplo.

Finalmente, dispara un montón de [links](http://www.onlamp.com/pub/a/onlamp/2004/05/06/writegreatcode.html) [para](http://www.gotw.ca/publications/concurrency-ddj.htm) [otros](http://www.joelonsoftware.com/articles/fog0000000319.html) [lados](http://en.wikipedia.org/wiki/Counting_sort) [también](http://lwn.net/Articles/82495/) [muy](http://en.wikipedia.org/wiki/Memoization) [interesantes](http://www.joelonsoftware.com/articles/APIWar.html)... en fin, aprendí un montón. :-)

Es lectura para un día lluvioso, ideal para revoleárselo a pibes que hacen Programación II en una facultad.

[http://www.shlomifish.org/philosophy/computers/optimizing-code-for-speed/](http://www.shlomifish.org/philosophy/computers/optimizing-code-for-speed/)

Saludos  
Marcelo  
PD: Es más, veo que [el sitio mismo del autor](http://www.shlomifish.org/) tiene bastantes artículos similares sobre computación... será cuestión de agendar el sitio y leerlo de a poco.  
PD2: Ignacio: habla de lo "malo" y lo "bueno" que puede llegar a ser el inlining de funciones, vos que sos un defensor tan acérrimo :-P . También habla de la pobre performance de algunas llamadas de la Glib, cosa que vos también nos demostraste en su momento con el Quicksort.  
