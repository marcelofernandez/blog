---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2010-02-06T18:04:28+00:00"
guid: https://blog.marcelofernandez.info/?p=525
title: PostgreSQL <strike>8.5</strike> 9.0 - Replicación mejorada
url: /2010/02/postgresql-9-0-streaming-replication/

---
Para los que no están enterados, [Streaming Replication](http://wiki.postgresql.org/wiki/Streaming_Replication) es la nueva **gran** característica de PostgreSQL 9.0 (ex-8.5), todavía en desarrollo. Estoy muy contento por la noticia, realmente era algo pendiente ver integrado algo de esto en [PostgreSQL](http://www.postgresql.org) mismo (ya que hay productos y/o [versiones modificadas](http://slony.info/) para hacer esto, pero no es lo mismo que "el original", claro está :-) ) y lo hace cada vez más adecuado para evitar (o al menos dejar a uno la opción de) utilizar motores de bases de datos muy buenas pero caras y propietarias (Oracle) o lamentablemente en problemas políticos/de gestión (MySQL).

Básicamente SR permite que exista un proceso "sender"en el master que envíe a procesos "receiver" en el/los nodos secundarios porciones de [WAL (Write-Ahead Log)](http://en.wikipedia.org/wiki/Write-ahead_logging), es decir, de transacciones "comiteadas" recientemente. Antes (PostgreSQL 8.4 y anteriores) los WAL se podían archivar a otro nodo una vez se hayan completado 16 MB de transacciones (por defecto), con lo cual si se tenía una BD que "cambie poco", 16 MB podían representar un lapso de tiempo físico importante. A partir de ahora, el lapso de tiempo se reduce a unos pocos segundos de diferencia en el master y la/las réplicas (dependiendo el enlace, la carga del master, etc.), con lo cual es una mejora substancial en las capacidades y posibilidades de PostgreSQL.

Hay que resaltar que por ahora este proceso es **asincrónico**, y aunque hay usuarios que pueden necesitar replicación sincrónica, cada esquema tiene sus [ventajas y desventajas](http://wiki.postgresql.org/wiki/Replication,_Clustering,_and_Connection_Pooling), y sin lugar a dudas esta primera versión de momento atiende muchísimas necesidades.  Combinado con otra de las novedades de 9.0 que es la posibilidad de usar un nodo secundario (recibiendo WALs mediante SR o el método tradicional) como Sólo Lectura (característica llamada " [Hot Standby](http://developer.postgresql.org/pgdocs/postgres/hot-standby.html)"), **9.0 da un paso muy adelante con respecto a versiones anteriores**. :-)

Para más adelante (y [planificado para hacerse](http://wiki.postgresql.org/wiki/Streaming_Replication#Future_release), ojo) quedó la opción de Straming Replication sincrónico y obtener más y mejor info del proceso de replicación ( _[lag](http://es.wikipedia.org/wiki/Lag)_, estadísticas, etc.). Otro punto flojo, que espero se resuelva mejor (o con un mecanismo estándar), es que uno tiene que scriptear a mano el [Failover](http://en.wikipedia.org/wiki/Fail_over), es decir, el proceso de recuperación ( [heartbeat](http://www.linux-ha.org/Heartbeat) es una herramienta genérica para este tipo de cosas).

Dejo algunos links relacionados con más info y demostraciones:
[http://www.depesz.com/index.php/2010/02/01/waiting-for-9-0-streaming-replication/](http://www.depesz.com/index.php/2010/02/01/waiting-for-9-0-streaming-replication/) [http://www.depesz.com/index.php/2010/01/08/waiting-for-8-5-hot-standby/](http://www.depesz.com/index.php/2010/01/08/waiting-for-8-5-hot-standby/)

¡Saludos!
Marcelo
