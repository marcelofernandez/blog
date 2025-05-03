---
author: Marcelo
category:
  - codear
  - python
  - ubuntu-ar
date: "2008-06-07T14:33:00+00:00"
guid: https://blog.marcelofernandez.info/?p=103
title: Apache 3.0 (?)
url: /2008/06/apache-30/

---
En mi carpeta de documentos "para leer" (o sea, pendientes) tenía una presentación que bajé hace algún tiempo de [la última ApacheCon](http://www.eu.apachecon.com/eu2008/) realizada en Amsterdam, en Abril de este año. Dicha presentación está muy buena, para [bajarla](http://roy.gbiv.com/talks/200804_REST_ApacheCon.pdf) y leerla atentamente: cuenta qué se cocina para la próxima gran versión de [Apache](http://httpd.apache.org/), nuestro [Servidor Web](http://es.wikipedia.org/wiki/Servidor_web) amigo :-)

[![](http://3.bp.blogspot.com/_nDZ247g0qSM/SEqju_5zzPI/AAAAAAAABFI/FwQRwQHpRks/s400/eu_2008_logo.jpg)](http://3.bp.blogspot.com/_nDZ247g0qSM/SEqju_5zzPI/AAAAAAAABFI/FwQRwQHpRks/s1600-h/eu_2008_logo.jpg)  
En fin, parece que Apache 3.0 se renovará completamente, y aunque aclara que lo expresado no está 100% consensuado y aprovado por la comunidad, [Roy Fielding](http://roy.gbiv.com/) es miembro de [Apache Fundation](http://www.apache.org/), esta charla fue la keynote del evento... entonces, por qué no tomarlo como muy probable que ocurra lo que nos cuenta?

En resumen, Apache 3.0 parece que va a tener:  

- Un modelo de proceso por cada plataforma: Chau elegir entre prefork,worker, etc. Va a haber un modelo "óptimo" por plataforma. Esto se lo llama el "spooning model".  

- También se va a quitar el soporte para plataformas que "apesten" (cuáles serán? :-D ). Para ellas, "usen Apache 1.3, y sino, tienen el código fuente".  

- No hay más opciones en tiempo de compilación.
- Reemplazo de la [APR](http://apr.apache.org/) por otra librería (Moccasin): no va a haber compatibilidad para atrás (a nivel de módulos, claro).
- Menos configuración: configuración default simple y segura, para programadores web (arrancar usando user/group del usuario, escuchar en un puerto alto como 8080, ejecución como usuario normal con parámetros, etc.).
- Introducción del protocolo [Waka](http://en.wikipedia.org/wiki/Waka_%28protocol%29), como futuro reemplazo de HTTP 1.x (!). Obviamente, HTTP se sigue soportando (va a seguir siendo el estándar Web por largo rato), aunque la novedad es la extensión del soporte a Waka también.

En resumen, para la comunidad y el proyecto en sí, los objetivos son:  

- Renovarse, "introduce some fresh thinking".
- Divertirse! ("Have Fun").
- Y lo más importante: atraer nuevamente a la comunidad alrededor del proyecto.  

Bueno, eso, ya que no leí nada sobre esto en ningún lado, lo posteo. :-)

Link al pdf: [http://roy.gbiv.com/talks/200804\_REST\_ApacheCon.pdf](http://roy.gbiv.com/talks/200804_REST_ApacheCon.pdf)  
Link al video: [http://streaming.linux-magazin.de/events/apacheconfree/archive/rfielding/frames-java.htm](http://streaming.linux-magazin.de/events/apacheconfree/archive/rfielding/frames-java.htm)

Como opinión personal, creo que ya son varios los "viejos" proyectos FLOSS que se renuevan (Python, Perl, KDE, Gnome lo está considerando...), y esto es más que importante para todos.  
Es como decir, "bueno, con todo lo anterior llegamos hasta acá; ahora, vamos por la gloria". Se vienen tiempos de nuevos y mejores aires para el FLOSS? Espero que sí. :-)

Saludos!  
Marcelo
