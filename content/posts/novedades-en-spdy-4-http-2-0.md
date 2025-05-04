---
author: Marcelo
category:
  - codear
  - sysadmin
  - web
date: "2013-05-26T15:38:30+00:00"
guid: https://blog.marcelofernandez.info/?p=1298
title: Novedades en SPDY/4, HTTP/2.0
url: /2013/05/novedades-en-spdy4-http2-0/

---
Si bien SPDY/4 todavía [está en desarrollo](https://github.com/grmocg/SPDY-Specification/blob/gh-pages/draft-mbelshe-spdy-00.txt), por lo que se deduce [de este hilo](https://groups.google.com/d/msg/spdy-dev/EWEEWSYtlhc/usK342C5g_EJ) y por lo que puede apreciarse en la forma que tienen [ambos](http://http2.github.io/http2-spec/ "HTTP/2.0 HTML draft") [drafts](http://grmocg.github.io/SPDY-Specification/draft-mbelshe-spdy-00.html "SPDY/4 specification draft"), éste va a dejar de ser un trabajo separado de HTTP/2.0 como sucedía hasta la versión anterior, sino que será un protocolo que implementará progresivamente lo que se vaya definiendo y ganando adhesiones en la discusión que se está teniendo en [el marco del IETF](http://lists.w3.org/Archives/Public/ietf-http-wg/ "Httpbis WG IETF list archives").

Es por esto que además de los cambios previamente definidos para SPDY/4, se le suma un "port" o "sincronización" de características definidas desde HTTP/2.0 hacia SPDY/4.  Los elementos que "se mudan" para desplazar a aquellos definidos en SPDY/3 cuentan con un relativo acuerdo general dentro del HTTPbis WG para ser parte de HTTP/2.0, y son:

- SYN\_STREAM, SYN\_REPLY, HEADERS pasan a ser reemplazados por HEADERS+PRIORITY\* y HEADERS. Ya había un frame de HEADERS en SPDY/3, pero no se usaban en casos comunes; ahora pasan a eliminarse los frames SYN\_STREAM y SYN\_REPLY, unificando todas las formas de envío de headers HTTP (ya sea para iniciar/responder peticiones) en el frame de HEADERS que ya existía (entiendo que sólo se le agrega un campo de prioridad). En resumen, en SPDY/4 (y en HTTP/2.0), las peticiones se pedirán y responderán con un frame de HEADERS.
- Unificaron los frames de RST\_STREAM y GOAWAY. Eran redundantes, claramente.
- Después de ciertas pruebas ( [ejemplo](http://lists.w3.org/Archives/Public/ietf-http-wg/2013JanMar/0929.html))(\*\*), ya está definido el _magic byte_ para inicializar una conexión directamente en HTTP/2.0 (o SPDY/4): "FOO \* HTTP/2.0\\r\\n\\r\\nBAR\\r\\n\\r\\n". Pasando en limpio, la idea es que (en principio) haya 3 mecanismos de uso de HTTP/2.0:

  - Vía TLS+NPN, como SPDY/3. De todas maneras, NPN se envió hace unos meses al TLS WG y lo [modificaron levemente](http://www.imperialviolet.org/2013/03/20/alpn.html), dándole forma a lo que se llama ALPN. Para el caso, hará lo mismo que NPN. Claro está, esto funciona sobre SSL/TLS (puerto 443), con sus pros y contras.
  - Si el servidor con el que hablamos no sabemos si soporta HTTP/2.0, se intentaría un upgrade mediante el mecanismo estándar de Upgrade de HTTP, como se hace con [WebSockets](http://en.wikipedia.org/wiki/WebSocket#WebSocket_protocol_handshake) ( [ejemplo](http://blogs.msdn.com/b/interoperability/archive/2012/11/02/more-http-2.0-prototyping.aspx)). Esto funciona sobre puerto 80, en claro.
  - Si el servidor con el que hablamos sí sabemos de antemano que soporta HTTP/2.0, directamente se envía el _magic byte_ luego de establecer la conexión TCP, para comenzar a intercambiar frames HTTP/2.0. La idea es ahorrar el tiempo (valioso) de upgrade. El _magic byte_ está elegido en base a que si lo enviamos por equivocación a un server que sólo soporta HTTP/1.1, éste cause el cierre de la conexión inmediatamente.
  - Hay otras propuestas para ahorrarse el tiempo de upgrade sin saber de antemano si el server soporta 2.0, como [publicar el soporte de HTTP/2.0 vía DNS](http://lists.w3.org/Archives/Public/ietf-http-wg/2013JanMar/1336.html)(y así el navegador podría saber si el server habla 2.0 al momento de hacer la resolución DNS), pero por el momento [la propuesta](http://tools.ietf.org/html/draft-lear-httpbis-svcinfo-rr-01) está en stand-by.
- Incluirá control de flujo por stream (además del control de flujo por sesión de SPDY/3).
- Un algoritmo de compresión de encabezados no basado en el stream completo de éstos, sino basado en cambios (diferencias entre los headers), para evitar los ataques del estilo [CRIME](https://www.imperialviolet.org/2012/09/21/crime.html "CRIME attack"). Sobre esto se está trabajando en el HTTPbis WG (principalmente por cosas como el requisito del mantenimiento de estado) y hay dos algoritmos con posibilidades ( [HeaderDiff](http://tools.ietf.org/html/draft-ruellan-headerdiff-00 "HeaderDiff spec") y [Delta](http://tools.ietf.org/html/draft-rpeon-httpbis-header-compression-03)). SPDY/4 incluiría este último.

\*: Este frame está en discusión en estos momentos, quizá se defina un nuevo frame PRIORITY para soportar repriorización "al vuelo" de streams.
\*\*: Es interesante comentar que en las pruebas de búsqueda del _magic byte_ realizadas se intentó encontrar un stream de bytes que haga que la gran mayoría de web servers actuales que soporten HTTP/1.1 cierren la conexión inmediatamente o al menos devuelvan un código HTTP de error (4xx). La idea es que si un cliente envía ese stream de bytes, éste se asegure de una respuesta satisfactoria que asegure que el server es HTTP/2.0, y en caso contrario (por error, al suponer que el server soporta 2.0 pero no es así), el efecto producido en el server (y en la comunicación en sí) sea el mínimo posible.

Vamos a ver cómo evoluciona, y si SPDY/4 se hace "estable" en un futuro próximo para poder ir haciendo pruebas. Pero parece ser claro que va a ser bastante diferente a SPDY/3.

Saludos
