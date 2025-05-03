---
author: Marcelo
category:
  - codear
  - python
  - ubuntu-ar
  - web
date: "2009-03-13T00:45:00+00:00"
guid: https://blog.marcelofernandez.info/?p=127
title: Novedades en Firefox 3.1 beta (será 3.5)
url: /2009/03/novedades-en-firefox-31-beta-sera-35/

---
[![](http://2.bp.blogspot.com/_nDZ247g0qSM/Sbm9wUh5i0I/AAAAAAAACMA/hZ19rzmamJk/s320/Firefoxlogo2.png)](http://www.mozilla.org/) Si bien la mayoría de los sitios de noticias tiene un enlace a [esta página](https://developer.mozilla.org/devnews/index.php/2009/03/12/firefox-31-beta-3-now-available-for-download/), detallando las novedades más importantes y generales en la aplicación, me gustaría destacar algunas de las novedades "bajo el capó" del próximo Firefox 3.5, que se describen en [esta otra página](https://developer.mozilla.org/en/Firefox_3.1_for_developers) ("Firefox para desarrolladores web").

Veamos, las que me parecen más interesantes son:  

- Soporte para los nuevos tags [audio](http://www.whatwg.org/specs/web-apps/current-work/#audio) y [video](http://www.whatwg.org/specs/web-apps/current-work/#video) del futuro (y en borrador todavía) [HTML 5](http://www.whatwg.org/specs/web-apps/current-work/) [incluído](https://developer.mozilla.org/En/Using_audio_and_video_in_Firefox). Lo interesante es que no sólo [permite controlar la reproducción](https://developer.mozilla.org/En/Using_audio_and_video_in_Firefox#Controlling_media_playback) por Javascript (algo relativamente lógico), sino que también permite que [Javascript procese y manipule la imagen mediante canvas](https://developer.mozilla.org/En/Manipulating_video_using_canvas). ¡ [Muy bueno](http://www.0xdeadbeef.com/weblog/?p=1093)! Un detalle es que por ahora soporta el formato [WAV](http://es.wikipedia.org/wiki/WAV) y [OGG](http://www.xiph.org/ogg/) únicamente.  

- Soporte para [descargar "al vuelo" una fuente](https://developer.mozilla.org/en/CSS/@font-face) [True Type](http://es.wikipedia.org/wiki/TrueType) u [Open Type](http://en.wikipedia.org/wiki/Open_Type) para ver una página exactamente igual a como el diseñador lo quiso. Esta funcionalidad respetará la nueva política de Control de Acceso para sitios con nombre diferente al de origen (ver más abajo).  

- ¡ [Multithreading](http://es.wikipedia.org/wiki/Multihilo_%28CPU%29) [en Javascript mismo](https://developer.mozilla.org/En/Using_web_workers)! Los navegadores están cada vez más cerca de convertirse en un Sistema Operativo... ahora vamos a tener un manejador de eventos para notificar de las cosas que nuestros hilos (workers) hagan, y sólo se puede acceder al DOM desde el "proceso" principal, tal como los [toolkits](http://www.gtk.org/) [gráficos](http://www.wxwidgets.org/) [para](http://www.eclipse.org/swt) [interfaces](http://es.wikipedia.org/wiki/Swing_%28biblioteca_gr%C3%A1fica%29) [de escritorios](http://es.wikipedia.org/wiki/Interfaz_gr%C3%A1fica_de_usuario) de hoy en día. :-)  
Lo bueno (?) es que son hilos reales, al nivel del Sistema Operativo (o al menos eso dice), no son hilos "emulados" ( [green threads](http://en.wikipedia.org/wiki/Green_threads)).
- Unas cuantas mejoras en [CSS](http://es.wikipedia.org/wiki/CSS): soporte de [estilos para diferentes medios](https://developer.mozilla.org/En/CSS/Media_queries) de entrada/salida (por ejemplo, según el tipo de dispositivo de entrada y cantidad de colores, resolución y relación de aspecto del medio de salida, etc.), [transformaciones](https://developer.mozilla.org/En/CSS/Using_CSS_transforms).
- Corrección de color [ICC](http://en.wikipedia.org/wiki/ICC_profile) [habilitada por defecto](https://developer.mozilla.org/En/ICC_color_correction_in_Firefox).
- Posibilidad de hacer [prefetching de DNS](https://developer.mozilla.org/En/Controlling_DNS_prefetching) desde el mismo HTML que está siendo renderizado. Esto (usado en forma medida), puede hacer que al hacer click en un link de la página que estamos viendo, Firefox ya haya resuelto de antemano vía DNS la IP del servidor de la nueva página, logrando que los tiempos de "saltos" entre links se acorten.
- Mejoras en el "Gran Objeto AJAX" ( [XmlHttpRequest](http://en.wikipedia.org/wiki/Xmlhttprequest)), como la posibilidad de saber [cuánto falta para finalizar la transferencia](https://developer.mozilla.org/En/Using_XMLHttpRequest#Monitoring_progress); y además, un relax en la política del " [mismo origen](https://developer.mozilla.org/en/Same_origin_policy_for_JavaScript)" para Javascript, [permitiendo hacer requests a otros servidores con dominio diferente al de origen](https://developer.mozilla.org/En/HTTP_access_control), siguiendo las directivas de un nuevo [Control de Acceso](http://dev.w3.org/2006/waf/access-control/) (que por lo que leí muy brevemente trabaja a nivel de encabezados HTTP).

Bueno, son bastantes cambios, esto sumado a [TraceMonkey](https://wiki.mozilla.org/JavaScript:TraceMonkey) seguramente va a hacer que Firefox esté preparado para las aplicaciones web de acá a unos años; aunque pensándolo bien, yo iría tachando la palabra "web" de la frase anterior, dejando "aplicaciones" a secas...

¡Saludos!  
Marcelo
