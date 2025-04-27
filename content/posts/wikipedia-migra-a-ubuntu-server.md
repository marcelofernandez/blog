---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2008-10-10T13:25:00+00:00"
guid: https://blog.marcelofernandez.info/?p=109
title: Wikipedia migra a Ubuntu Server
url: /2008/10/wikipedia-migra-a-ubuntu-server/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/SO9iWKSWZvI/AAAAAAAABO8/TiXHeU0Tloo/s200/Wiki.png)](http://es.wikipedia.org/) Según [este artículo](http://www.computerworld.com/action/article.do?command=viewArticleBasic&articleId=9116787&intsrc=news_ts_head), la [Fundación Wikimedia](http://www.wikimedia.org/), entidad que mantiene online a la [Wikipedia](http://es.wikipedia.org/), decidió migrar sus servidores de una combinación de [Red Hat Linux](http://www.redhat.com/) y [Fedora](http://fedoraproject.org/) a [Ubuntu Server](http://www.ubuntu.com/products/WhatIsUbuntu/serveredition) (8.04 LTS, para ser más precisos).

Quiero destacar algunos puntos de la noticia:  

- 400 servidores soportan un tráfico de 684.000.000 visitantes al año (1.873.972 promedio por día, 1301 visitantes por minuto). Viendo su [página de estadísticas](http://en.wikipedia.org/wiki/Wikipedia:Statistics), esta semana, estuvo teniendo picos de 55.000 hits por segundo. Y ojo, que esto es sólo Wikipedia, los servidores migrados sirven a todos los sites de la Fundación.

 [![](http://1.bp.blogspot.com/_nDZ247g0qSM/SO9dLkBptMI/AAAAAAAABO0/slnRqVnHtmI/s400/reqstats-weekly.png)](http://1.bp.blogspot.com/_nDZ247g0qSM/SO9dLkBptMI/AAAAAAAABO0/slnRqVnHtmI/s1600-h/reqstats-weekly.png)

- Sólo 5 personas alrededor del mundo administran semejante infraestructura.
- La migración empezó en 2006 (reemplazando [Proxys Reversos](http://es.wikipedia.org/wiki/Proxy#Reverse_Proxy) perimetrales) y ahora van por los [Application Servers](http://en.wikipedia.org/wiki/Application_server).
- Buscaban estandarizar, simplificar, unificar toda la infraestructura. Está claro para un sysadmin no es lo mismo escribir scripts, instalar paquetes, configurar servicios en un Red Hat 4 que un Red Hat 5, que un Fedora 8... etc.  

- El caos se generó cuando en sólo 18 meses aumentaron la cantidad de servidores de 15 a 200 (!). Es que, cuando montaron los primeros servidores, RedHat tenía una distribución libre y gratuita, y sólo se pagaba el soporte (si se lo necesitaba). Pero la empresa del sombrero rojo cambió de opinión (creo que por 2003), comenzando a cobrar por la adquisición del producto en cualquier forma diferente del código fuente, y creando una distribución "comunitaria" (aunque financiada por ellos) llamada Fedora. Es por esto que Wikimedia terminó con un mix de Red Hats y Fedoras. Ahora van hacia Ubuntu en busca de ahorro de costos (instalar Ubuntu Server no cuesta dinero, sólo hay que pagar si se quiere soporte de Canonical) y menos trabajo (que también se traduce en un ahorro de costos indirectamente).  

Como conclusión, más allá de que sea una buena noticia para los ubunteros como yo (y por esta vez), creo que es una buena noticia para todos aquellos que se deciden por instalar software libre. El valor agregado del software libre no está en ahorrar costos al momento de adquirir el software, ya que esto puede o no ser posible, según las circunstancias y las necesidades. El valor agregado que sólo el software libre puede brindar es la libertad que uno tiene de cambiar de proveedor, ya que el software es tanto propiedad del que lo utiliza como de todos.

Sí, hoy me levanté "predicador"... y qué? :-)

Saludos  
Marcelo
