---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
  - web
date: "2008-12-31T17:33:00+00:00"
guid: https://blog.marcelofernandez.info/?p=122
title: Xplico, un decodificador de tráfico de Red
url: /2008/12/xplico-un-decodificador-de-trafico-de-red/

---
Gracias al [rincón de Laramies](http://laramies.blogspot.com/) me sigo enterando de cosas novedosas acerca de herramientas y noticias sobre seguridad informática; esta vez de la existencia de [Xplico](http://www.xplico.org/), un software para analizar y disecar \[1\] capturas de red en formato [PCap](http://wiki.wireshark.org/Development/LibpcapFileFormat) (el más común, utilizado por herramientas como [Wireshark](http://www.wireshark.org/) o [Tcpdump](http://www.tcpdump.org/), por ejemplo).

Su objetivo es detectar el protocolo de aplicación (no basado en el número puerto, sino que lo hace [interpretándolo](http://taosecurity.blogspot.com/2006/09/port-independent-protocol.html) [realmente](http://spid.wiki.sourceforge.net/)), y extraer la información relevante (el _stream_ de datos), mostrarla y darle a uno la posibilidad de guardarla aparte para abrirla con un software que pueda reproducir dicho formato. Por ejemplo, puede detectar el formato [SIP](http://es.wikipedia.org/wiki/Session_Initiation_Protocol)(utilizado comúnmente en llamadas telefónicas de [VoIP](http://es.wikipedia.org/wiki/Voz_sobre_IP)) en una serie de paquetes capturados y guardarlo aparte para poder escuchar la llamada telefónica. :-)

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/SVutc5UDtzI/AAAAAAAAB1Y/04uyU1jyaB0/s400/voip_sip.png)](http://4.bp.blogspot.com/_nDZ247g0qSM/SVutc5UDtzI/AAAAAAAAB1Y/04uyU1jyaB0/s1600-h/voip_sip.png) De todas maneras, su página de [estado del proyecto](http://www.xplico.org/status) nos dice que le falta soporte de varios protocolos más (por ahora soporta decentemente HTTP, SMTP, IMAP, POP, FTP, [IPP](http://es.wikipedia.org/wiki/Protocolo_de_Impresi%C3%B3n_en_Internet), [PJL](http://en.wikipedia.org/wiki/Printer_Job_Language) y SIP, entre otros) pero insisto que es un proyecto [muy interesante](http://www.xplico.org/roadmap), con muchos usos y muy útiles. Además, sería un lindo como add-on para Wireshark, IMHO.

Vean las [capturas de pantalla](http://www.xplico.org/screenshot).

Saludos  
Marcelo  
\[1\]: Es "disecar", no "disectar"; es la primera acepción de [esta palabra](http://buscon.rae.es/draeI/SrvltConsulta?TIPO_BUS=3&LEMA=disecar). La segunda acepción es la conocida. :-)
