---
author: mfernandez
category:
  - codear
  - linux
  - programación
  - sysadmin
  - ubuntu-ar
date: "2009-07-04T16:16:56+00:00"
guid: https://blog.marcelofernandez.info/?p=247
title: PostgreSQL 8.5
url: /2009/07/postgresql-8-5/

---
No, no me equivoqué de número de versión. :-) [![pgsql_logo](/wp-content/uploads/2009/07/pgsql_logo.png)](http://www.postgresql.org)

Si bien [PostgreSQL 8.4 ya salió](http://www.postgresql.org/about/news.1108) y tiene [unas cuantas novedades](http://www.postgresql.org/about/press/features84), quisiera [hacerme eco](http://www.depesz.com/index.php/2009/07/03/waiting-for-8-5-lets-start/) de lo que está en la cola de parches a aprobar para integrar la nueva rama de desarrollo de PostgreSQL 8.5. [Esta lista](http://commitfest.postgresql.org/action/commitfest_view?id=2) está compuesta por cosas como:

- [**GRANT ON ALL IN schema**](http://archives.postgresql.org/message-id/4A4DE104.8090605@pjmodos.net): Una manera de otorgar permisos a todos los objetos de un esquema.
- [**Machine-readable explain output v2**](http://archives.postgresql.org/message-id/603c8f070906172043w6dba5b18x60e1cea76c44dba0@mail.gmail.com): Salidas del comando [EXPLAIN](http://www.postgresql.org/docs/8.3/interactive/using-explain.html) fácilemente legibles por otro software, es decir, fácilmente parseables para su procesamiento. El comando explain aceptaría como segundo parámetro el formato (además de los actuales) la keyword FORMAT, quedando así: EXPLAIN FORMAT \[TEXT \| JSON \| XML\]. Está bueno porque abre la puerta del desarrollo de herramientas de auto-tunning, basado en reglas estilo Sistema Experto.
- [**SE-PostgreSQL**](http://wiki.postgresql.org/wiki/SEPostgreSQL): Security-Enhanced PostgreSQL, una mejora sustancial en la seguridad del SGBD y en el nivel de detalles del sistema de privilegios.
- [**Replicación Sincrónica**](http://wiki.postgresql.org/wiki/NTT%27s_Development_Projects#Synch_Rep): Si algo le faltaba a PostgreSQL es una manera de replicación sincrónica integrada en el gestor. Esperemos que esto sí entre, aunque está [bastante](http://archives.postgresql.org/message-id/3f0b79eb0906152313n7d566aa8u80c73516453e5777@mail.gmail.com) [discutido](http://archives.postgresql.org/message-id/603c8f070907021902ufbb62cj7ca435f9d8712b4d@mail.gmail.com) el asunto porque es un cambio relativamente "gordo" en los internals del código, pero por lo que leí de estas discusiones parece haber consenso en llevarlo adelante e implementarlo. Por mi parte... ¡lo necesito ya! :-)
- [**\\dL para listas los lenguajes utilizados en psql**](http://archives.postgresql.org/pgsql-hackers/2009-07/msg00158.php): Es una nueva característica en [psql](http://www.postgresql.org/docs/8.3/interactive/app-psql.html), la de poder listar los lenguajes instalados en la base de datos (plpythonu, plpgsql, etc.).
- [**Estadísticas de esperas por bloqueos**:](http://archives.postgresql.org/message-id/497CFC84.2070302@paradise.net.nz) La idea es poder tener un historial de los bloqueos sufridos por los clientes en la BD, para poder detectar problemas y saber bien qué pasa en todo momento.

Bueno, de más está decir que no hay ninguna garantía de que todo esto sea finalmente integrado en 8.5, sino que se está evaluando al mejor estilo Software Libre **™**: [peer-review](http://es.wikipedia.org/wiki/Revisi%C3%B3n_por_pares), discusión de estrategias del proyecto, consenso en la comunidad de desarrolladores y posterior aceptación/pedido de mejoras/rechazo. Así que veremos cómo sigue.

Saludos!
Marcelo

# \\dL for languages
