---
author: mfernandez
category:
  - codear
  - programación
  - ubuntu-ar
date: "2007-09-09T13:48:00+00:00"
guid: https://blog.marcelofernandez.info/?p=82
title: Administrador SQLite - SQLiteman
url: /2007/09/administrador-sqlite-sqliteman/

---
Buenas... hace algún tiempo comenzamos a utilizar en desarrollos en estado "inicial" Bases de Datos [SQLite](http://www.sqlite.org/)... ya sea porque [Python tiene mucha facilidad](http://docs.python.org/lib/module-sqlite3.html) para crear y laburar con este tipo de BD, porque nos parece rápida y de sencilla administración, y porque para hacer pruebas... es bárbara. En fin, con SQLite, creás tu BD en un toque y te ponés a laburar.  

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/RuP7vJaDcPI/AAAAAAAAAKw/txL5pWceiAs/s400/sqlite.gif)](http://1.bp.blogspot.com/_nDZ247g0qSM/RuP7vJaDcPI/AAAAAAAAAKw/txL5pWceiAs/s1600-h/sqlite.gif)  

El tema es que si bien uno tiene siempre a mano el administrador de línea de comandos ("sqlite3") para hacer querys, ver la descripción de alguna que otra tabla, etc., muchas otras veces (más que nada cuando quiero hacer querys complejos, como joins de varias tablas) siento la falta de algún administrador SQLite pero en formato [GUI](http://es.wikipedia.org/wiki/IGU).... un montón de veces busqué y nunca encontré nada que me gustara.

Necesitaba algo con Soporte Linux + Paquetes Ubuntu (que sea sencillo de instalar), además de ser gratis (preferentemente Open Source, claro). Si tenía que perder tiempo en compilar en algunas de las PCs de desarrollo (mi casa, el trabajo, etc.), era más fácil invertir el tiempo en hacer el deploy de otra BD ( [PostgreSQL](http://www.postgresql.org/)) con el [PGAdmin](http://www.pgadmin.org/), je.

Por suerte, gracias a [GetDeb](http://www.getdeb.net/), me encontré con [SQLiteMan](http://www.getdeb.net/app.php?name=Sqliteman) ( [Website](http://sqliteman.com/)). Un caño, simple de instalar, simple de usar, 100% efectivo. Y todos felices. :-)

Screenshot Obligada (Usa el QT 4.3 de mi Feisty con el look and feel "Cleanlooks", que imita el Clearlooks de GTK):

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/RuP975aDcQI/AAAAAAAAAK4/WXPUD_Kx9u4/s400/Pantallazo-collection.db+-+Sqliteman.png)](http://4.bp.blogspot.com/_nDZ247g0qSM/RuP975aDcQI/AAAAAAAAAK4/WXPUD_Kx9u4/s1600-h/Pantallazo-collection.db+-+Sqliteman.png)  


Saludos  
Marcelo
