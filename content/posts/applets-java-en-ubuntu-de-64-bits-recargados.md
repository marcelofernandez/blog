---
author: Marcelo
category:
  - codear
  - linux
  - programación
  - ubuntu-ar
date: "2009-07-18T23:55:44+00:00"
guid: https://blog.marcelofernandez.info/?p=259
title: Applets Java en Ubuntu de 64 bits recargados
url: /2009/07/applets-java-en-ubuntu-de-64-bits/

---
[![java_logo](/wp-content/uploads/2009/07/java_logo.png)](http://www.java.com) Ayer actualicé mi Ubuntu 9.04 (64 bits) y vi que había una actualización de los paquetes " [sun-java6](http://packages.ubuntu.com/search?keywords=sun-java6&searchon=names&suite=jaunty&section=all)", que corresponden a la [Máquina Virtual Java](http://es.wikipedia.org/wiki/M%C3%A1quina_virtual_Java). Para mi sorpresa, se trata de una actualización bastante importante de todos los paquetes de la [implementación](http://java.sun.com/javase/) distribuida por [Sun](http://www.sun.com) de Java (que aún no es lo mismo que el proyecto [OpenJDK](https://openjdk.dev.java.net/)), y que incluye (después de [¡6 años de espera!](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4802695)) una implementación del [Java Plugin de Sun](http://packages.ubuntu.com/jaunty/sun-java6-plugin) para 64 bits.

Lo bueno es que ya puedo probar la [nueva versión del plugin Java](http://blogs.sun.com/javaone2008/entry/applets_reloaded) (incluida a partir de Java 6 Update 10) sobre Linux x86-84 en forma totalmente nativa.

Acá hay un video de un técnico de Sun, Kenneth Russell, donde describe todas las nuevas características del Java Plugin "recargado":

La presentación del mismo en pdf está [acá](http://developers.sun.com/learning/javaoneonline/2008/pdf/TS-6290.pdf). Para información detallada y ejemplos para correr en el navegador, vayan a [este link](https://jdk6.dev.java.net/plugin2/). Las novedades son muchas e importantes, y aunque la gente de Sun debería haberle dado importancia a esto hace tiempo, mejor tarde que nunca; les sugiero pegarle una mirada.

Saludos!
