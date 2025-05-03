---
author: Marcelo
category:
  - codear
  - python
  - web
date: "2012-07-15T15:57:22+00:00"
guid: https://blog.marcelofernandez.info/?p=1187
title: Investigando el protocolo SPDY
url: /2012/07/investigando-el-protocolo-spdy/

---
Un tiempo atrás venía buscando áreas de investigación para estudiar, y me encontré con una interesante propuesta de Google, de renovar el ya "viejito pero cumplidor" protocolo [HTTP 1.1](http://tools.ietf.org/html/rfc2616 "RFC 2616: HTTP/1.1"), llamada [SPDY](http://dev.chromium.org/spdy/spdy-whitepaper "SPDY Whitepaper") (no sin algo de sentido comercial, se nota).

De ahí en adelante (dado que el desarrollo [es abierto](http://groups.google.com/group/spdy-dev "SPDY-Dev Group") a la discusión en general) me dediqué [a profundizar en él](http://dev.chromium.org/spdy/spdy-protocol/spdy-protocol-draft3 "SPDY v3 Protocol Specification"), entender sus ventajas (lo cual implica entender algunas cosas feas de HTTP 1.1 y la Web de hoy en día), limitaciones, y cosas que faltan implementar. Me apasionó el tema, tanto es así que lo propuse como tema de Tesis para mis estudios y hasta ahora vengo bien (bien con las promesas a mi Director, claro está :-P).

Mi idea era con este post abrir una serie de artículos para documentar lo que voy aprendiendo sobre este protocolo, que cada vez tiene más _hype_ en la industria, tanto que [Twitter](http://www.genbeta.com/redes-sociales/twitter-adopta-spdy-por-defecto-en-los-navegadores-que-lo-soportan "Twitter adopta SPDY") y Google ya lo implementan en sus servidores ( [Facebook está en camino](http://lists.w3.org/Archives/Public/ietf-http-wg/2012JulSep/0251.html "Facebook HTTP2 Expression of Interest")), mientras que Chrome/Chromium y Firefox ( [Opera se está sumando](http://dev.opera.com/articles/view/opera-spdy-build/ "Opera SPDY release")) también lo usan si está disponible.

Personalmente me puse a probarlo y a tratar de implementarlo usando Python, [forkeando un proyecto](https://github.com/marcelofernandez/python-spdy "Python-Spdy on Github") que ya existía y arreglando los problemas más obvios que encontré. Todavía tengo todo el código "atado con alambre", no bien testeado, y no estoy seguro si funciona del todo (jua!), pero de ahora en más voy a tratar de mejorarlo y mantenerlo mientras pueda, además de dejar algún rastro por aquí y por mi trabajo de investigación formal.

En resumen, el IETF draft de SPDY [está disponible acá](http://tools.ietf.org/html/draft-mbelshe-httpbis-spdy-00 "IETF Draft - SDPY v3"); tanto parece estar movilizando este protocolo, que el [HTTPbis Working Group](http://trac.tools.ietf.org/wg/httpbis/trac/wiki/Http2Proposals "HTTP 2 Proposals"), encargado de definir un futuro HTTP 2.0, se está moviendo desde hace un tiempo para discutir las propuestas de SPDY. Y esto recién empieza...

Les dejo un video del último Google IO 2012 que es un excelente acercamiento técnico al tema:
{{< youtube id="zN5MYf8FtN0" >}}

Saludos
