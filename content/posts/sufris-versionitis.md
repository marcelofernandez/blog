---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2007-07-01T15:38:00+00:00"
guid: https://blog.marcelofernandez.info/?p=71
title: Sufrís "versionitis"?
url: /2007/07/sufris-versionitis/

---
Una de las cosas que hacen estable y previsible a una distribución como Ubuntu es que una vez que fue lanzada una versión definitiva, las actualizaciones de los programas que contiene [sólo se limitan a correcciones de seguridad o errores críticos](https://wiki.ubuntu.com/StableReleaseUpdates). Esto está bueno, si uno quiere que todo "sólo funcione" una vez se configuró y la PC se comenzó a usar diariamente; lo mismo pasa si se usa la PC para laburar y necesita que todo ande de forma ordenada y sin sobresaltos.

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/RofSLY2NBHI/AAAAAAAAAHg/LbiCpQ7KA0c/s400/getdeb_logo.png)](http://4.bp.blogspot.com/_nDZ247g0qSM/RofSLY2NBHI/AAAAAAAAAHg/LbiCpQ7KA0c/s1600-h/getdeb_logo.png)

Sin embargo, muchas veces uno navega por la web y lee cómo los distintos programas se van actualizando y van incluyendo funcionalidades interesantes, necesarias y hasta divertidas. Y ahí está uno para decir "yo quiero eso!"... y tiene que conformarse a esperar 6 meses por "eso". :-D

Por suerte encontré [www.getdeb.net](http://www.getdeb.net/), un sitio creado por gente interesada en brindar últimas versiones de paquetes para Ubuntu (para 32 y 64 bits), muy ameno y en castellano. No estaría posteando esto si no hubiera probado ningún paquete, pero me instalé el [Pidgin](http://pidgin.im/) ( [ex Gaim](http://es.wikipedia.org/wiki/Pidgin_%28software%29#Historia)), el último [DeVeDe](http://www.rastersoft.com/programas/devede_es.html) (programa para hacer DVDs muy sencillo), y sólo fue bajar, doble click y listo. GDebi, el programa gráfico para instalar paquetes .deb "solitarios" se encarga de todo.

El sitio está muy bien organizado (por categorías), y ni tiene paquetes "a lo loco" ni está vacío. Considero que con la cantidad que hay, está "bien", ya que un usuario nuevo no se marea (como puede pasar con el Synaptic). También tiene un [feed RSS](http://es.wikipedia.org/wiki/Rss), para que lo agregue a mi [Google Reader](http://reader.google.com/) y ver todos los días qué novedades hay. Cada paquete tiene una página "propia", donde se puede ver una captura de pantalla del programa, y algún comentario de algún usuario, donde se pueden hacer preguntas o comentar inconvenientes que se tuvo al usar/instalar (no se olviden, algunas versiones son de desarrollo, o en etapa de "pruebas").

Por último, también permite "filtrar" para qué versión de Ubuntu estoy buscando paquetes (Dapper, Edgy y Feisty en sus versiones de 32 y 64 bits). Todo bastante completito, bien presentado y explicado, con el "look and feel" de las páginas web de Ubuntu.

Lo bueno es que esto acerca mucha más gente que no sabe compilar\* a probar las versiones de desarrollo de los programas, involucrarse con la comunidad, y proveer feedback a los desarrolladores de software libre.

Como una de las formas de colaborar según la página de colaboración es haciéndose eco de este esfuerzo... acá está, muchachos. :-D

Gracias por el gran esfuerzo que hacen y ojalá sigan así.

\\* dije compilar porque es, en mi opinión, el principal obstáculo de los usuarios nuevos al querer testear o utilizar software en desarrollo.Saludos!
MarceloActualización: Instalé también el [deb del nspluginwrapper](http://www.getdeb.net/release.php?id=861) para poder (por fin!) usar el Firefox de 64 bits en vez del de 32 bits (que usaba porque Adobe no lanzó todavía una versión de 64 bits del plugin de Flash)... y funciona 10 puntos, justo [como dice la página](http://www.getdeb.net/release.php?id=861):

1 -Descargar el .tar.gz [desde el sitio de Adobe](http://www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash&promoid=BIOW).
2 - Extraer el archivo libflashplayer.so a /usr/lib/mozilla/plugins/
3 - Abrir la terminal y ejecutar (como usuario, sin 'sudo'):

```
nspluginwrapper -i /usr/lib/mozilla/plugins/libflashplayer.so
```

Aclaración: El paso 3 debe seguirse para cada usuario del sistema.
