---
author: Marcelo
category:
  - codear
  - sysadmin
  - ubuntu-ar
date: "2008-03-27T23:45:00+00:00"
guid: https://blog.marcelofernandez.info/?p=99
title: PF - Packet Filter Firewall
url: /2008/03/pf-packet-filter-firewall/

---
Estoy leyendo un poco de doc del PF de [OpenBSD](http://www.openbsd.org/) ( [FreeBSD](http://www.freebsd.org) y [NetBSD](http://www.netbsd.org) también lo incluyen, entre otros).

No sé si [Iptables/Netfilter](http://www.netfilter.org/) tiene o no tal o cual característica (diría que no, al menos las incluídas en el core como sí las tiene PF), pero me atrevo a decir que este firewall (PF) es MUY GROSO. Pero groso en serio, además de tener reglas mucho más legibles (que es importante cuando uno tiene más de 10).

Doc principal:  
[http://www.openbsd.org/faq/pf/index.html](http://www.openbsd.org/faq/pf/index.html)

Doc Extra:  
[http://home.nuug.no/~peter/pf/en/](http://home.nuug.no/%7Epeter/pf/en/)

Presentación del creador (ojo, es viejo, del 2003):  
[http://www.benzedrine.cx/sucon/](http://www.benzedrine.cx/sucon/)

Para no decirles que lean solamente (je), les comento cosas que me sorprendieron hasta ahora (no terminé de leer!):  

- Soporte de pfsync y CARP: pfsync sincroniza reglas y [CARP](http://en.wikipedia.org/wiki/Common_Address_Redundancy_Protocol) es un protocolo de Redundancia, para armar un cluster como Firewall (!!).
- Soporte de Syn Proxy: Básicamente no pasa los "Syn" al servidor que está detrás hasta tanto la conexión no esté establecida; luego los forwardea, claro, manteniendo el estado de la conexión (cosa que hace solito en las últimas versiones).
- Soporte de [QoS](http://en.wikipedia.org/wiki/QoS) con AltQ (Netfilter también lo tiene con módulos aparte aunque vienen en todas las distros). En [FreeBSD](http://www.freebsd.org/) para agregar esto hay que recompilar el kernel (ufa).
- IP Normalization (scrubbing) y IP Modulation: Normaliza paquetes "irregulares"; es decir, los "maquilla" para que sean todos coherentes. Corrige campos con valores no del todo aleatorios (por deficiencias del host origen). Acá se me escapó un poco la tortuga, mete cosas para pulir los paquetes fragmentados y después filtrarlos como corresponde.
- Optimización de reglas: digamos que las procesa y arma [árboles de decisión](http://en.wikipedia.org/wiki/Decision_tree) internos (para darse una idea). Esto hace que si un paquete no matchea con un nodo del árbol, descarta esa rama entera para evaluar.
- El concepto de tablas (listas de direcciones [CIDR](http://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)) para luego [hashearlas](http://en.wikipedia.org/wiki/Hash_table) (supongo) está bueno.
- PF permite rutear paquetes diferentes a lo que diga la tabla de enrutamiento (!!!).
- Labura con varios enlaces sin problemas (permite redundancia o clasificación del tráfico) sin hacer malabares, no? (siempre le tuve miedo a [http://lartc.org](http://lartc.org/), je).
- Fingerprinting pasivo del SO integrado, listo para ser usado en las reglas que uno arme :-) (ojo, considerar que no es 100% efectivo, claro está).


Y ojo al configurarlo, que la última regla que matchea un paquete, gana (a diferencia del resto del mundo). De esto me di cuenta jugando primero y leyendo después (mal hecho) :-)

Seguramente voy a agregar más a medida que vaya leyendo. Si me estoy perdiendo alguna, no me peguen. :-)

Disclaimer: Ojo, no soy sysadmin full time "hiper-profesional"; quizás un partidario de Netfilter me diga "eh! pero iptables hace xxx cosa y le da 20 vueltas!!" y tenga razón. Aún así, estas cositas de PF me llaman la atención.

Saludos!  
Marcelo

PD: Alguien sabe cómo priorizar los ACK de subida con iptables? Me refiero a esto (está implementado con PF):  
[http://www.benzedrine.cx/sucon/mgp00033.html](http://www.benzedrine.cx/sucon/mgp00033.html)  
[http://www.benzedrine.cx/sucon/mgp00034.html](http://www.benzedrine.cx/sucon/mgp00034.html)
