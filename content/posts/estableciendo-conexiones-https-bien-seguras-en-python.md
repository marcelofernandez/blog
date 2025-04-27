---
author: mfernandez
category:
  - codear
  - programación
  - python
  - ubuntu-ar
date: "2011-02-19T19:45:55+00:00"
guid: https://blog.marcelofernandez.info/?p=1112
title: Estableciendo conexiones HTTPS "bien seguras" en Python
url: /2011/02/estableciendo-conexiones-https-bien-seguras-en-python/

---
Hace unos días que tenía pendiente colgar esto acá, ya que a alguien en [PyAr](http://www.python.org.ar) [le fue útil](http://comments.gmane.org/gmane.org.user-groups.python.argentina/44953).

[HTTPS](http://es.wikipedia.org/wiki/Https) es la manera de establecer conexiones HTTP pero _seguras_, en el sentido de que previo al diálogo HTTP estándar pero luego de establecerse la conexión TCP contra el servidor, se negocia entre los participantes una conexión/sesión "especial" entre ambos.  Allí se intercambian certificados con el fin de autenticar contra quién se "está hablando", para luego, si hubo éxito en la dicha comprobación, encriptar (o no) todo lo que va para el otro lado, tanto del Servidor al Cliente (generalmente un navegador), como del Cliente al Servidor.

Todo eso forma parte de SSL 3.0 (hoy [TLS 1.0](http://es.wikipedia.org/wiki/Transport_Layer_Security)), y si bien se puede utilizar para cualquier conexión TCP (SMTP, IMAP, etc., lo pueden usar también), su uso más común se da cuando uno entra a su casilla de Webmail o su cuenta del Banco desde el navegador;  lo que sucede allí es que nuestro navegador autentica al Servidor, y si todo va bien nos muestra el famoso "candadito" e informa que la sesión "es segura". Ahora bien, en esos casos, el Banco o Webmail no nos autentica a nosotros como Cliente y deja que cualquiera se conecte a su página, ya que eso requiere varios pasos más (sería realmente engorroso que sea obligatorio).

Sin embargo, en nuestra vida de programadores nos solemos encontrar con necesidades del entorno que nos obliguen a esta situación un tanto extrema y bastante más segura: que tanto el Cliente que desarrollamos autentique al Servidor como que el Servidor autentique al Cliente, amén de que la conexión muy probablemente deberá estar encriptada. **Esto nos permitirá asegurarnos que los Clientes (software) que se conecten a nuestro Servidor sean únicamente quienes queremos que sean** (o casi).

Python, dentro del módulo [httplib](http://docs.python.org/library/httplib.html) nos provee de la clase **HTTPSConnection**, que maneja y nos abstrae en varias de estas cuestiones de SSL/TLS y nos deja trabajar a nivel HTTP (una capa más arriba). Lo que hay que observar bien (y que recién en la versión 2.7 de la documentación apareció en rojo), es que esta clase aún cuando uno le puede pasar como parámetro los paths a certificados, **en realidad no hace ninguna comprobación de validez del certificado que el Servidor exporta**. Con lo cual, hay un potencial problema: que nuestro cliente mande los datos a cualquier lado menos a nuestro Servidor de confianza.

¿Y cómo se fuerza a que nuestro programa Cliente chequee el certificado del Servidor? Armé esta clase que extiende sólo lo necesario a HTTPSConnection, que funciona en Python 2.6.x y que me permite hacer eso.

Le agregué un ejemplo en la llamada a \_\_main\_\_ para mostrar cómo se usa:

```
#!/usr/bin/env python
#-*- coding: utf-8 -*-

import socket
import ssl
import httplib

class HTTPSClientAuthConnection(httplib.HTTPSConnection):
    """ Class to make a HTTPS connection, with support for full client-based
        SSL Authentication.
    """

    def __init__(self, host, port, key_file, cert_file, ca_file, timeout=None):
        httplib.HTTPSConnection.__init__(self, host, key_file=key_file,
                                                   cert_file=cert_file)
        self.key_file = key_file
        self.cert_file = cert_file
        self.ca_file = ca_file
        self.timeout = timeout

    def connect(self):
        """ Connect to a host on a given (SSL) port.
            If ca_file is pointing somewhere, use it to check Server Certificate.

            Redefined/copied and extended from httplib.py:1105 (Python 2.6.x).
            This is needed to pass cert_reqs=ssl.CERT_REQUIRED as parameter
            to ssl.wrap_socket(), which forces SSL to check server certificate
            against our client certificate.
        """
        sock = socket.create_connection((self.host, self.port), self.timeout)
        if self._tunnel_host:
            self.sock = sock
            self._tunnel()
        # If there's no CA File, don't force Server Certificate Check
        if self.ca_file:
            self.sock = ssl.wrap_socket(sock, self.key_file, self.cert_file,
                        ca_certs=self.ca_file, cert_reqs=ssl.CERT_REQUIRED)
        else:
            self.sock = ssl.wrap_socket(sock, self.key_file, self.cert_file,
                                              cert_reqs=ssl.CERT_NONE)

if __name__ == '__main__':
    # Little test-case of our class
    import sys
    if len(sys.argv) != 6:
        print 'usage: ./https_auth_handler.py host port key_file cert_file ca_file'
        sys.exit(1)
    else:
        host, port, key_file, cert_file, ca_file = sys.argv[1:]
    conn = HTTPSClientAuthConnection(host, port, key_file=key_file,
                                           cert_file=cert_file, ca_file=ca_file)
    conn.request('GET', '/')
    response = conn.getresponse()
    print response.status, response.reason
    data = response.read()
    print data
    conn.close()
```

En cuanto a la generación de las claves de Cliente, Servidor y CA [hay unos cuantos artículos](http://www.google.com.ar/search?q=openssl+certificate+client+server+creation) y es relativamente sencillo una vez que se entiende qué se está haciendo. Por otra parte que en estos casos es muy común que el Servidor exija que el Cliente si o sí envíe su certificado o sino la conexión se deberá caer; esto en el caso del Servidor Apache está [bien documentado](http://httpd.apache.org/docs/2.2/ssl/ssl_howto.html#allclients).

Entiendo que en Python 3.2 esto está resuelto en la misma API standard de Python (en su momento abrí [un ticket similar](http://bugs.python.org/issue3466) para usar esto con urllib2, que incluye cosas importantes como el manejo de cookies, por ejemplo), y comentaron eso.

Según me dijeron en PyAr, es casi seguro que este ejemplo no funcione en Python 2.7, así que en un futuro no muy lejano espero poder adaptar este código a dicha versión y seguramente lo estaré subiendo en esta página.

Saludos
