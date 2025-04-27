---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2008-01-15T02:22:00+00:00"
guid: https://blog.marcelofernandez.info/?p=96
title: 'PortBunny: Escáner de Puertos Rápido!'
url: /2008/01/portbunny-escaner-de-puertos-rapido/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/R4wZ_L79vkI/AAAAAAAAAQM/wp3B_zrIbgI/s400/logoS.png)](http://www.recurity-labs.com/portbunny/portbunny.html)[Port Bunny](http://www.recurity-labs.com/portbunny/portbunny.html) es un [escáner de puertos](http://es.wikipedia.org/wiki/Esc%C3%A1ner_de_puertos), que si bien al principio (nobleza obliga) mucho crédito no le dí (¿cómo un port scanner en kernel mode??), luego de ver atentamente el [video de su presentación](http://dewy.fem.tu-ilmenau.de/CCC/24C3/matroska/24c3-2131-en-port_scanning_improved.mkv) (sí, son 44 minutos 100% interesantes!), más [los slides](http://www.recurity-labs.com/portbunny/24c3PortBunnySlides.pdf), sinceramente aprendí un montón de [TCP](http://es.wikipedia.org/wiki/TCP), [NMap](http://insecure.org/nmap/) (el escáner de referencia), cómo funciona un port scanner por dentro, y por supuesto, de Port Bunny mismo, je.

Y qué tiene Port Bunny de nuevo con respecto al todopoderoso NMap? Bueno (miren el video), esta gente hace un estudio pormenorizado de cómo circula el tráfico por la red, cuál es la medida de óptima (más rápida) de llevar a cabo el escaneo de puertos (sin perder paquetes), y por último el problema de falta de timing que presentan los hosts que filtran o dropean paquetes apropósito (o no).

Entonces, se plantea que si bien [TCP ya resuelve esto](http://en.wikipedia.org/wiki/Transmission_Control_Protocol#Congestion_control) mediante su control de congestiones, un port scanner no clasifica, ya que los Syn scans y otros scans (salvo el connect) no llegan a ser conexiones completas TCP: el control de congestión se hace sobre el tráfico de la conexión ya establecida, no sobre un "medio-inicio", "medio-fin", un "ack", etc.

Sin embargo, la idea de llevar estas técnicas implementadas en TCP al nivel de un port scanner, se logra basándose en que al menos un puerto del host que está siendo escaneado nos va a responder correctamente con un ACK (ya que está abierto), por lo que el control de tiempo y congestión se realiza insertando en el escaneo un paquete "con éxito de vuelta", llamado trigger, cada cierta cantidad N de pruebas ("probes"), para tratar de optimizar el uso de la red entre los hosts y terminar el escaneo lo antes posible, con óptimos resultados.

Está claro que acá no se habla de ser stealth (que los IDSs/IPSs no te detecten), sino que el objetivo es no desperdiciar tiempo en el port scanning; saber cuál es la carga del enlace hasta el destino y qué tiempo tenemos hasta ahí para esperar o no por una respuesta.

Si los confundió, perdon, miren el video y me cuentan. Ah, por cierto, según sus estadísticas, PortBunny le pasa el trapo por bastante a Nmap (tarda mucho menos en completar el escaneo) , así que puede estar bueno probarlo y ver si los resultados son los mismos. Al menos en teoría, no encuentro elementos para creer lo contrario...

Me faltaba, es Open Source (GPLv2), y sólo para Linux (no dice qué versión, requiere los headers del kernel para insertarlo como módulo), y su uso parece ser muy sencillo.

Saludos!  
Marcelo

PD: Gracias al [Blog de Laramie](http://laramies.blogspot.com/) (no, no son [los cigarros](http://en.wikipedia.org/wiki/Laramie_Cigarettes#Laramie_Cigarettes), je), que cuando postea algo de seguridad, está buenísimo. URL: [http://laramies.blogspot.com/](http://laramies.blogspot.com/)
