---
author: mfernandez
category:
  - codear
  - linux
  - tests
  - ubuntu-ar
date: "2007-11-08T03:10:00+00:00"
guid: https://blog.marcelofernandez.info/?p=88
title: Adobe Reader 8.1 para Linux
url: /2007/11/adobe-reader-81-para-linux/

---
[![](http://2.bp.blogspot.com/_nDZ247g0qSM/RzKPGrJ7VLI/AAAAAAAAANU/vrdh-MgAvOo/s400/AdobeReader8.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/RzKPGrJ7VLI/AAAAAAAAANU/vrdh-MgAvOo/s1600-h/AdobeReader8.png) Si, si, ya sé que para leer simplemente un [archivo pdf](http://es.wikipedia.org/wiki/Pdf) [Evince](http://www.gnome.org/projects/evince/) y/o [Kpdf](http://kpdf.kde.org/) funcionan bien... pero... y si me llega algún pdf "raro", que aprovecha las características nuevas del formato? (uso de formularios, anotaciones, encriptación, etc.)\[1\]

En fin... me enteré hace poquito que [Adobe](http://www.adobe.com/) (muy silenciosamente) lanzó su versión 8.1 del "acroread", es decir, del [Adobe Reader](http://www.adobe.com/products/reader/) para Solaris/Sparc y GNU/Linux/i386. Con este programa tenemos máxima compatibilidad al leer y trabajar con estos archivos.

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/RzKOobJ7VKI/AAAAAAAAANM/OhLVtHK4CSc/s400/Pantallazo-BlackHat_2007_MetaSploit_tactical_paper.pdf+-+Adobe+Reader.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/RzKOobJ7VKI/AAAAAAAAANM/OhLVtHK4CSc/s1600-h/Pantallazo-BlackHat_2007_MetaSploit_tactical_paper.pdf+-+Adobe+Reader.png) Lo bueno:  

- Anda rapidísimo, vuela comparado con la antigüa versión 7 (siempre sobre Linux); sigue utilizando [GTK](http://es.wikipedia.org/wiki/Gtk) como toolkit gráfico.
- Eliminaron el modo [MDI](http://es.wikipedia.org/wiki/Interfaz_de_m%C3%BAltiples_documentos), que nunca fue soportado por GTK (o sea, lo habían metido "a la fuerza"), con lo cual la versión 7 era un hack horrible al laburar con varios documentos. Ahora está todo en [SDI](http://es.wikipedia.org/wiki/Interfaz_de_documento_%C3%BAnico). :-)  

- Hay más comunicación con el equipo de desarrollo y el usuario: Adobe creó un [blog sobre Acroread para Unix/Linux](http://blogs.adobe.com/acroread/). Bien, al menos ahora hay una sección exclusiva para los que usamos otros SO.  

- El software en general está muy mejorado, bonito, y muyusable (aunque para ser justos, tampoco estamos hablando de una aplicación compleja como un CAD). Diría que podría dejarlo como default para mi sistema.  

- [Proveen paquetes binarios](http://www.adobe.com/products/acrobat/readstep2_allversions.html) para instalarlo muy fácilmente: .rpm, .deb y .tar.gz.

Lo Malo:  

- Todavía está en inglés (y [Francés, Alemán, Japonés](http://blogs.adobe.com/acroread/2007/10/adobe_reader_811_for_french_ge.html)... pero en Español, todavía no).
- Usando [Compiz](http://es.wikipedia.org/wiki/Compiz) en Ubuntu 7.10, los efectos de transición a pantalla completa funcionan pero no del todo bien, y los deshabilité. Tiene algunos defectos gráficos muy pavos y pequeños, se ve que algunas cosas siguen siendo medio 'hack' sobre GTK para obtener máxima compatibilidad (será?).
- Me gustaría que se pudiera regular la cantidad de líneas que se baja al darle a la ruedita del mouse, se me hace que es poco.  

Lo Feo:  

- Sigue siendo código cerrado (por qué, Adobe? Qué ganan?). Eso sí, es gratuito.  

- Por lo tanto, no viene en ninguna distribución Linux por defecto.
- Además, para hacerlo andar en mi arquitectura AMD64 (los binarios son para 386), tuve que bajar el .tar.gz y ejecutar el script INSTALL que viene adentro (después me avivé que estaba el .deb, pero siempre para i386, con lo cual habrá que hacer un 'dpkg -i --force-architecture acroread\_xxxx.deb').
- Y por último, tuve el problemita de que me pedía la librería libgtkmozembed.so \[2\] (en forma opcional), para lo cual tuve que bajar el [XulRunner](http://www.mozilla.org/projects/xul/xre.html) de [Mozilla para Linux 386](http://developer.mozilla.org/en/docs/XULRunner_1.8.0.4_Release_Notes) ( [enlace](http://ftp.mozilla.org/pub/mozilla.org/xulrunner/releases/1.8.0.4/linux-i686/en-US/xulrunner-1.8.0.4.en-US.linux-i686.tar.gz)), descomprimirla en algún lado (yo elegí '/usr/local/xulrunner32/') y decirle al Adobe Reader en sus preferencias que lo busque en esa ruta.  

Otra Captura:

 [![](http://1.bp.blogspot.com/_nDZ247g0qSM/RzKRGbJ7VNI/AAAAAAAAANg/6niNbLCi06w/s400/Pantallazo.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/RzKRGbJ7VNI/AAAAAAAAANg/6niNbLCi06w/s1600-h/Pantallazo.png)  
Comparando cómo renderiza el mismo documento cada uno (Poppler vs. Adobe), va desde algo mejor a mucho mejor el software de Adobe, tanto en calidad (el hinting de las fuentes es mejor), como en la aparición de algunos "artefactos" muy sutiles (por Poppler). Soy un neófito en cuando a teoría de fuentes, pero poniendo un documento a lado del otro, se nota.

Saludos!  
Marcelo

\[1\] En una búsqueda (muy corta) no encontré algún documento que resumiera lo no implementado con respecto al último estándar por [Poppler](http://poppler.freedesktop.org/), la librería que interpreta los PDFs para Evince y Kpdf. Sin embargo, puedo afirmar que no implementa todas las características del estándar Pdf 1.7. [Acá](http://www.acrobatusers.com/blogs/leonardr/history-of-pdf-openness/) hay un resumen de la historia.  
\[2\]: [En las FAQ del Blog de Adobe](http://blogs.adobe.com/acroread/2007/09/adobe_reader_811_faqs.html) está explicado qué es, para qué sirve y como se busca si no es detectado automáticamente. Aclaro de nuevo, es opcional; el programa funciona sin hacer esto, sólo sale un cartelito más o menos molesto diciendo lo que no encuentra.  
