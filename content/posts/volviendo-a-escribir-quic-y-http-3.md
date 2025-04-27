---
author: mfernandez
category:
  - codear
  - personal
  - web
date: "2023-01-04T13:33:16+00:00"
guid: https://blog.marcelofernandez.info/?p=1680
tag:
  - http
  - http/3
  - quic
  - redes
  - web
title: Volviendo a Escribir - QUIC y HTTP/3
url: /2023/01/volviendo-a-escribir-quic-y-http-3/

---
De a poquito y con el reset del año, vuelven las ganas de leer y aprender sobre temas nuevos para escribir algo de texto y código como output de lo aprendido. En este caso, y para complementar [aquello que escribí sobre HTTP/2](http://www.labredes.unlu.edu.ar/sites/www.labredes.unlu.edu.ar/files/site/data/tyr/http2_Fernandez_Tolosa.pdf), estoy preparando algo similar pero sobre QUIC como protocolo de transporte y HTTP/3 como protocolo de aplicación sobre él.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/HTTP-1.1_vs._HTTP-2_vs._HTTP-3_Protocol_Stack.svg/640px-HTTP-1.1_vs._HTTP-2_vs._HTTP-3_Protocol_Stack.svg.png)

Ambos son Internet Standard desde hace muy poco tiempo ( [quic](https://www.rfc-editor.org/rfc/rfc9000.html), [http/3](https://www.rfc-editor.org/rfc/rfc9114.html)) y vienen y/o dependen de un paquete de otros protocolos y especificaciones que [no son poca cosa](https://quicwg.org/).

Aún así, mi idea es describir sus conceptos fundamentales en español, como para que lo entienda un estudiante de una asignatura de redes de una Universidad cualquiera; analizar sus pros y cons, y por último dejar algo de código de ejemplo como para poder entender desde el punto de vista de un programador dónde tiene lugar para hacer un aporte y dónde no.
