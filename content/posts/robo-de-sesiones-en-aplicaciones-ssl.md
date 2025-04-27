---
author: mfernandez
category:
  - uncategorized
date: "2008-12-24T20:57:00+00:00"
draft: "true"
guid: https://blog.marcelofernandez.info/?p=118
title: Robo de Sesiones en Aplicaciones SSL
url: /

---
En la última [DefCon](http://www.defcon.org/), la [número 16](http://www.defcon.org/html/defcon-16/dc-16-post.html), hubo una charla que [llamó la atención](http://it.slashdot.org/article.pl?sid=08/09/09/1558218) porque otra vez, el tema era el [robo de sesiones](http://en.wikipedia.org/wiki/Session_hijacking); en este caso (y nuevamente), utilizando [GMail](https://www.gmail.com): " [_Automated HTTPS Cookie Hijacking_](http://fscked.org/blog/fully-automated-active-https-cookie-hijacking)" fue el título de la charla brindada por [Mike Perry](http://fscked.org/about). Sus **investigaciones le permiten descartar como 100% efectivo el mecanismo de encriptación (vía [TLS/SSL](http://en.wikipedia.org/wiki/Transport_Layer_Security)) del tráfico web en las aplicaciones online para evitar el [SideJacking](http://erratasec.blogspot.com/2007/08/sidejacking-with-hamster_05.htmldef)** (conocido masivamente en 2007 gracias a  [Robert Graham](http://www.erratasec.com/about.html)).

**SideJacking**

Para poder comprender mejor de dónde viene esto, el SideJacking consiste en:  

1. [Esnifear](http://en.wikipedia.org/wiki/Sniffer) en una red donde esté la víctima (o conseguir sus paquetes http de alguna manera).  (Sí, hay muchas maneras, je). Ahora supongamos que esta persona inicia o ya tiene iniciada una sesión en GMail, y capturamos este tráfico (entre mucho otro, pero que obviamente podemos descartar).  

1. Conseguir la [Cookie](http://en.wikipedia.org/wiki/HTTP_cookie) de sesión que inicialmente establece el servidor, y que va hacia el mismo en cada petición que hace la víctima al sitio en cuestión. Tener en cuenta que muchos sistemas web implementan SSL únicamente en el login y no en el resto del sitio \[1\], con lo cual no hace falta que capturemos el inicio de una sesión, sino que **es suficiente interceptar cualquier petición HTTP que haga la víctima** (una imagen, un CSS, un Javascript, etc.).  

1. Usar la cookie de sesión capturada para conectarse al sistema web, y listo, el servidor "piensa" que nosotros somos la víctima y nos muestra, por ejemplo, su "Bandeja de Entrada" de Gmail, y **podremos utilizar cualquier función del sistema web tal como si fuéramos la víctima**, ya que el servidor de allí en más no puede diferenciar un usuario del otro \[2\].  

Para el paso 1 y 2, Robert Graham utilizó la herramienta [Ferret](http://www.erratasec.com/ferret.html), y para el tercero, [Hamster](http://www.erratasec.com/research.html). Si bien se conocían de antemano estos puntos débiles de las Cookies, con la avance imparable de las redes wireless (y más aún sin encriptar o con encriptación débil como [WEP](http://en.wikipedia.org/wiki/Wired_Equivalent_Privacy)) más estas herramientas que [lo hacen extremadamente sencillo](http://www.youtube.com/watch?v=nFNFa-48lpI), el impacto "al público" [fue bastante importante](http://itmanagement.earthweb.com/secu/article.php/3694671).

La solución propuesta en ese momento fue la utilización de encriptación en algún nivel del stack TCP/IP, como VPN ( [IPSec](http://en.wikipedia.org/wiki/Ipsec), [OpenVPN](http://en.wikipedia.org/wiki/OpenVPN), etc.) o [WPA/WPA2](http://en.wikipedia.org/wiki/WPA2) (para conexiones Wireless) a nivel de capa 2, o en su defecto (y en la mayoría de los casos es posible asumir que así será) SSL contra la web que utilicemos, y en toda la sesión de usuario.

**Active HTTPS Cookie Stealing**

Resulta que, como bien [comentó en la lista de Bugtraq](http://www.securityfocus.com/archive/1/475658/30/0/threaded) hace un año, el bueno de Mike avisó que no sólo es imprescindible usar SSL para evitar el robo de la sesión, sino que no hay que olvidarse del flag "connection type" al momento de establecer las cookies. Este flag, configurado casi siempre como "cualquier conexión", permite que la cookie de sesión en cuestión sea utilizada tanto en conexiones HTTPS y también HTTP. Y aquí reside el problema: **la enorme mayoría de aplicaciones web implementan SSL intentando evitar el robo de sesiones, ¡pero permiten que las cookies puedan ser utilizadas en conexiones encriptadas y sin encriptar!**

¿Cómo se aprovecha esto? Fácil, gracias a herramientas como [CookieMonster](http://fscked.org/blog/cookiemonster-available-all-site-admins-bloggers-students), o [SurfJack](http://code.google.com/p/surfjack/). Lo que hacen, a grandes rasgos, es lo siguiente:

**¿Solución?**

¿Cuál es la solución? Más fácil aún, prendiendo el "bitito" en 1 de "secure connections" en la llamada a la función set\_cookie() del software que utilizamos en el server (obviamente, varía de un lenguaje a otro, pero es un detalle). Veamos, en PHP, la [definición de esta función](http://ar2.php.net/set_cookie):  

```
bool setcookie (string $name [, string $value<br />                    [, int $expire [, string $path <br />                    [, string $domain <br />                    [, bool $secure <br />                    [, bool $httponly]]]]]] )<br />[...]<br />secure<br />Indicates that the cookie should only be transmitted over a secure HTTPS connection from the client.<br />When set to TRUE, the cookie will only be set if a secure connection exists. The default is FALSE.<br />On the server-side, it's on the programmer to send this kind of cookie only on secure connection (e.g. with respect to $_SERVER["HTTPS"]). <br />
```

**Esto recién empieza**  
Lo más sorprendente (?) de todo es que, viendo que hay herramientas automatizadas, suficiente información para impersonar relativamente fácil cualquier sesión, hay mucho software sin corregir. Sin ir más lejos, recién esta semana se dió a conocer que Joomla, un proyecto muy importante en el mundo de los CMSs, sufre de este problema.   
Por último y para terminar, quiero agregar que si bien el Sistema de Home Banking que uso obviamente utiliza SSL en todos lados, **lamentablemente las cookies que establece son para cualquier tipo de conexión, abriendo una posibilidad de que alguien me "robe" una sesión HTTPS mía en mi caja de ahorro.**

\[1\]: Si bien puede parecer ilógico que los desarrolladores del sistema web no implementen SSL en un sitio (en parte o en forma total), la utilización de encriptación impacta en la escalabilidad dado el incremento de poder de procesamiento que su utilización implica. De hecho, hasta hoy, muchos servicios tan ampliamente utilizados como [Hotmail](http://www.hotmail.com) por ejemplo, no utilizan SSL o no redireccionan automáticamente su página principal a su versión encriptada (por ejemplo, que [http://www.hotmail.com](http://www.hotmail.com) redireccione automáticamente a [https://www.hotmail.com](https://www.hotmail.com), aunque dicho sea de paso, no existe); en cambio, tanto Gmail como [Yahoo! Mail](http://mail.yahoo.com) redireccionan y utilizan SSL sólo en el login (por defecto).  

\[2\] Esto es así porque las cookies le sirven al servidor para asociar una sesión a todo el conjunto de peticiones HTTP que pueda hacer un único usuario, y proviene de una incapacidad del protocolo HTTP. Como sabrán, [HTTP es un protocolo sin estado](http://en.wikipedia.org/wiki/HTTP#HTTP_session_state) ( _stateless_), por lo cual el cliente hace una petición (o varias, depende) sobre una conexión TCP y luego se cierra. Para permitir que un servidor "recuerde" al usuario luego de terminada una petición HTTP, los desarrolladores implementan estos mecanismos alternativos (como las Cookies y variables en las URLs), que si bien salvan el problema inicial, introducen otros como el motivo de este post.

**Enlaces:**

Sidejacking  

- Post inicial: [http://erratasec.blogspot.com/2007/08/sidejacking-with-hamster\_05.html](http://erratasec.blogspot.com/2007/08/sidejacking-with-hamster_05.html)
- Software: [http://www.erratasec.com/sidejacking.zip](http://www.erratasec.com/sidejacking.zip)
- Video Explicativo: [http://www.youtube.com/watch?v=nFNFa-48lpI](http://www.youtube.com/watch?v=nFNFa-48lpI)
- Paso a paso (Español): [http://vlan7.blogspot.com/2007/08/sidejacking-con-ferret-y-hamster-ataque.html](http://vlan7.blogspot.com/2007/08/sidejacking-con-ferret-y-hamster-ataque.html)  

http://enablesecurity.com/2008/08/11/surf-jack-https-will-not-save-you/

En su charla comenta que publicó esto en Bugtraq hace como un año  
http://it.slashdot.org/it/08/09/09/1558218.shtml  
http://www.defcon.org/  
http://www.defcon.org/html/defcon-16/dc-16-speakers.html#Perry  
http://seclists.org/bugtraq/2007/Aug/0070.html

Esto sí parece interesante; por lo poco que leí, parece que GMail y  
otros sitios SSL suelen guardar cookies (info de sesión, digamos) sin  
el  bit de "sólo por conexiones encriptadas", lo cual hace factible de  
robar dichas sesiones si capturamos algo de tráfico (el tipo lo avisó  
usando ToR)...

Voy a seguirlo leyendo, pero entiendo que es interesante.

Post:

[http://fscked.org/blog/fully-automated-active-https-cookie-hijacking](http://fscked.org/blog/fully-automated-active-https-cookie-hijacking)


Slides:

[http://fscked.org/talks/ActiveHTTPSCookieStealing.pdf](http://fscked.org/talks/ActiveHTTPSCookieStealing.pdf)

Video, directo de la DefCon 16:

[http://media.defcon.org/dc-16/video/dc16\_perry\_TOR/dc16\_perrry\_TOR.m4v](http://media.defcon.org/dc-16/video/dc16_perry_TOR/dc16_perrry_TOR.m4v)  

* * *



Joomla: Session hijacking vulnerability, CVE-2008-4122



References



[http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2008-4122](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2008-4122)

[http://int21.de/cve/CVE-2008-4122-joomla.html](http://int21.de/cve/CVE-2008-4122-joomla.html)

[http://enablesecurity.com/2008/08/11/surf-jack-https-will-not-save-you/](http://enablesecurity.com/2008/08/11/surf-jack-https-will-not-save-you/)

[https://www.defcon.org/html/defcon-16/dc-16-speakers.html#Perry](https://www.defcon.org/html/defcon-16/dc-16-speakers.html#Perry)



Description



When configuring a web application to use only ssl (e. g. by forwarding all

http-requests to https), a user would expect that sniffing and hijacking the

session is impossible.



Though, for this to be secure, one needs to set the session cookie to have the

secure flag. Else the cookie will be transferred through http if the victim's

browser does a single http-request on the same domain.



Joomla 1.5.8 does not set that flag. I've contacted the Joomla security team

in advance but got no reply.



Disclosure Timeline



2008-11-18: Vendor contacted

2008-12-16 Published advisory



* * *

Este es el mismo problema de seguridad que les mandé hace un tiempo, que fue motivo de una charla de la última DefCon (16):

[http://www.defcon.org/html/defcon-16/dc-16-post.html](http://www.defcon.org/html/defcon-16/dc-16-post.html)

La charla en particular es esta:

"Mike Perry - 365 Day: Active HTTPS Cookie Hijacking"

Slides:

[https://www.defcon.org/images/defcon-16/dc16-presentations/defcon-16-perry.pdf](https://www.defcon.org/images/defcon-16/dc16-presentations/defcon-16-perry.pdf)

Videos (no los ví):

[http://media.defcon.org/dc-16/video/dc16\_perry\_TOR/dc16\_perrry\_TOR\_full.mov](http://media.defcon.org/dc-16/video/dc16_perry_TOR/dc16_perrry_TOR_full.mov)

[http://media.defcon.org/dc-16/video/dc16\_perry\_TOR/dc16\_perrry\_TOR.m4v](http://media.defcon.org/dc-16/video/dc16_perry_TOR/dc16_perrry_TOR.m4v)

Coincidentally, Slashdot reports that Mike Perry has released the tool from this talk today. you can find the article here:

HTTPS Cookie Hijacking Not Just For Gmail

[http://it.slashdot.org/it/08/09/09/1558218.shtml](http://it.slashdot.org/it/08/09/09/1558218.shtml)

Por este problema en su Sistema de Home Banking le escribí hace como un mes al Banco Río (el banco donde tengo cuenta), a [seguridad@bancorio.com.ar](mailto:seguridad@bancorio.com.ar) y todavía no tuve respuesta, obviamente.

Evidentemente el problema está \*en casi todos lados, en todas las  
webapps\*, es algo que muy pocos saben, y que se soluciona sólo poniendo  
"true" en la típica llamada a la función de establecimiento de la  
cookie en tu lenguaje favorito.

Veamos, por ejemplo, PHP, la función set\_cookie():

bool setcookie  ( string $name  \[, string $value  \[, int $expire  \[,  
string $path  \[, string $domain  \[, bool $secure  \[, bool $httponly  
\]\]\]\]\]\] )

Noten el flag opcional $secure. Para más info, ver: [http://ar2.php.net/set\_cookie](http://ar2.php.net/set_cookie)

* * *



Ante todo me presento, mi nombre es Marcelo Fidel Fernández, soy  
cliente del Banco y trabajo como un Profesional Informático  
independiente, dado que soy Licenciado en Sistemas de Información. Si  
bien me apasionan muchos aspectos de la tecnología, uno de los que  
considero más interesantes es la de la Seguridad Informática.

Es así como estuve leyendo este sitio\[1\], donde se da a conocer una  
herramienta automatizada para la captura de sesiones HTTPS, junto con  
información de cómo trabaja la misma y algunas condiciones necesarias  
(como la de estar entre el cliente y el servidor con el fin de capturar  
el tráfico).

Al revisar si el Sistema de Home Banking del sitio establecía las cookies  
sólo para envío y utilización sobre conexiones únicamente encriptadas,  
me encuentro con la sorpresa de que \_no es así\_, con lo cual un ataque  
de este tipo (y cualquier otro similar) sería potencialmente exitoso.  
Esto lo pude comprobar ingresando a mi cuenta, y obteniendo del  
navegador Firefox información de las cookies guardadas por el sitio ' [santanderrio.com.ar](http://santanderrio.com.ar/)', tal como lo describe el artículo mencionado previamente.

Dada la relevancia de este problema potencial, me gustaría saber si pueden tomar algún tipo de medidas para que las cookies de sesión enviadas por el Sistema de Home Banking se establezcan exclusivamente para conexiones encriptadas (HTTPS).

Todos los lenguajes/frameworks de programación web permiten establecer esta opción en las cookies, como por ejemplo PHP, donde en su función set\_cookie(), permite establecerle el parámetro "secure"\[2\].

\[1\] [http://fscked.org/blog/fully-automated-active-https-cookie-hijacking](http://fscked.org/blog/fully-automated-active-https-cookie-hijacking)

\[2\] [http://ar2.php.net/set\_cookie](http://ar2.php.net/set_cookie)
