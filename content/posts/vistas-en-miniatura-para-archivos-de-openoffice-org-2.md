---
author: Marcelo
category:
  - codear
  - linux
date: "2006-12-28T12:57:00+00:00"
guid: https://blog.marcelofernandez.info/?p=37
title: Vistas en Miniatura para Archivos de OpenOffice.org 2
url: /2006/12/vistas-en-miniatura-para-archivos-de-openofficeorg-2/

---
Holas... buceando por la web, bajé algunos libritos electrónicos. Algunos estaban en formato [CHM](http://en.wikipedia.org/wiki/Microsoft_Compressed_HTML_Help), y algunos en [PDF](http://es.wikipedia.org/wiki/Portable_Document_Format).

Si bien Ubuntu trae software para leer ambos ( [Evince](http://www.gnome.org/projects/evince/) para PDFs y [XChm](http://xchm.sourceforge.net/) para los archivos .chm), me pareció piola cómo [Nautilus](http://www.gnome.org/projects/nautilus/) mostraba una "Vista Previa" de los archivos PDF (así como con las fotos, videos, etc.), pero no lo mostraba de los archivos chm. Google no me contestó, pero sí encontré [ésta página](http://doc.gwos.org/index.php/Thumbnails_Ooo2), donde se explica cómo hacer que Nautilus muestre las miniaturas de los archivos de OpenOffice (formato [OpenDocument](http://es.wikipedia.org/wiki/OpenDocument)):  

- .odt (Texto)
- .ods (Planilla de Cálculo)
- .odb (Base de Datos)
- .odp (Presentación)
- .odg (Gráficos)
- .odf (Fórmulas).

 [![](http://2.bp.blogspot.com/_nDZ247g0qSM/RZO_Qqt66lI/AAAAAAAAAB0/snIOX8-24Ho/s320/screenshot1.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/RZO_Qqt66lI/AAAAAAAAAB0/snIOX8-24Ho/s1600-h/screenshot1.png)

Este método también funciona para archivos de Openoffice.org 1.x creados o modificados con la versión 2. Como Openoffice.org 2 guarda una vista previa del documento en el archivo generado, todo lo que hace falta es extraer la imagen y guardarla con el nombre que Nautilus necesita.

Es sorprendente cómo este tipo de "detalles" no están solucionados siendo tan sencillo arreglarlo, ya que Openoffice.org es la suite ofimática por defecto de Ubuntu (y casi todos las distribuciones de Linux). Por lo menos [hay un Bug al respecto](https://bugs.launchpad.net/distros/ubuntu/+source/nautilus/+bug/25827) de esto en Ubuntu, donde hay más info y se puede participar. Allí mismo está [esta otra página](http://www.ubuntuforums.org/showthread.php?t=76566&page=3) que explica cómo hacer lo mismo.

Si alguien no lo entiende porque está en inglés, que deje el comentario y lo traduzco (aunque en realidad hay que seguir los pasos, copiar y pegar y listo).

Saludos!  
Marcelo  
PD: Ah, y me olvidaba: al final, alguien tiene idea de cómo hacer thumbnails de archivos CHM ?
