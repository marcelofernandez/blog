---
author: Marcelo
category:
  - codear
  - sysadmin
  - ubuntu-ar
date: "2009-05-24T21:09:00+00:00"
guid: https://blog.marcelofernandez.info/?p=132
title: PostgreSQL Conference 2009
url: /2009/05/postgresql-conference-2009/

---
Gracias a [este post](http://www.postgresql-es.org/node/291) del sitio de [PostgreSQL en español](http://www.postgresql-es.org/), me entero que terminó la conferencia anual de la comunidad de [PostgreSQL](http://www.postgresql.org/), llamada [PGCon 2009](http://www.pgcon.org/2009/). Obviamente, la nueva versión que está al caer ( [8.4](http://www.postgresql.org/developer/beta)) acaparó todo un track completo, así que [acá está la lista de charlas](http://www.pgcon.org/2009/schedule/events.en.html) para ver de qué se trató.

[![](http://3.bp.blogspot.com/_nDZ247g0qSM/Shm_CFnjq8I/AAAAAAAACVI/ER-w6tAP09s/s320/pgcon.png)](http://www.pgcon.org/) Particularmente, a mí me interesó mirar los slides de:  

- [Introducing Windowing functions](http://www.pgcon.org/2009/schedule/events/128.en.html): Es una nueva e importante característica de PostgreSQL 8.4, el soporte de consultas con varios niveles de aggregates, no sólo el primero con la cláusula GROUP BY, sino que también se pueden subdividir estos grupos (llamados Windows) y ejecutar funciones dentro de cada subrango. Muy interesante.
- [No More Waiting](http://www.pgcon.org/2009/schedule/events/179.en.html): La clásica y necesaria charla sobre las nuevas cosas de PostgreSQL 8.4.
- [SkyTools3: Replication (Londiste3)](http://www.pgcon.org/2009/schedule/events/134.en.html): Si hay algo que se le puede "recriminar" a PostgreSQL es la falta de replicación built-in (está prometido para la 8.5, tough). Sin embargo, hay muchas herramientas extra para hacerlo, y [Londiste](https://developer.skype.com/SkypeGarage/DbProjects/SkyTools) es una de ellas (desarrollada por la gente de [Skype](http://www.skype.com/), que usa PostgreSQL para todo).

En fin, PostgreSQL es una base de datos muy robusta y confiable, la vengo utilizando profesionalmente y nunca me dejó de a pie. Tiene una excelente [documentación](http://www.postgresql.org/docs/), una [licencia](http://www.postgresql.org/about/licence) extremadamente libre (BSD), casi todas las [características](http://www.postgresql.org/about/featurematrix) de las grandes, y un muy buen soporte en la lista de [PostgreSQL en español](http://archives.postgresql.org/pgsql-es-ayuda/).

Saludos  
Marcelo
