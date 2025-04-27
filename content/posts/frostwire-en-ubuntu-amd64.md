---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2007-12-25T00:33:00+00:00"
guid: https://blog.marcelofernandez.info/?p=93
title: Frostwire en Ubuntu AMD64
url: /2007/12/frostwire-en-ubuntu-amd64/

---
Hola!

Este post en realidad es casi una "copia" de un mail que envié a la lista de [Ubuntu Argentina](http://ubuntu-ar.org/), para darle una mano a alguien que quería hacer correr el ( [famoso?](http://www.frostwire.com/blog/?p=40)) programa de [P2P](http://es.wikipedia.org/wiki/P2P) [FrostWire](http://www.frostwire.com/) en Ubuntu pero en arquitectura AMD64.

El problema se reduce a que por dependencias adjuntas al paquete de 32 bits, hay que correr el programa con una [JVM](http://es.wikipedia.org/wiki/JVM) de 32 bits, por lo que puede aplicarse a otros programas con problemas parecidos.

Ok Matías, a ver, repasemos (yo también tengo Ubuntu 7.10 en un AMD64). Bajate el "Tarball Bundle" de acá:

[http://www.frostwire.com/?id=downloads](http://www.frostwire.com/?id=downloads)

y descomprimilo en, por ejemplo, "/home/matias/frostwire-4.13.4.noarch"

El paquete a instalar es "ia32-sun-java6-bin", que con el comando "dpkg -L ia32-sun-java6-bin" podemos ver qué archivos contiene (y su ubicación). Se puede ver que lo instala en "/usr/lib/jvm/ia32-java-6-sun", y el binario de ejecución está en "/usr/lib/jvm/ia32-java-6-sun/bin".

Ahora, lo siguiente lo haces todo en la misma ventana/pestaña de la terminal (también llamada consola) de Ubuntu.

Lo que vamos a hacer es, teniendo instaladas las 2 versiones de Java (la de 32 bits y la de 64), hacer que frostwire use la de 32, diciendo que el PATH es diferente al que tenés por defecto (el PATH es la variable donde se van a buscar los comandos que ejecutás).

Si abrís una terminal, y ejecutás directamente "java -version", te devuelve la versión de 64 bits:

```
marcelo@saturno:~/src/frostwire-4.13.4.noarch$ java -version
java version "1.6.0_03"
Java(TM) SE Runtime Environment (build 1.6.0_03-b05)
Java HotSpot(TM) 64-Bit Server VM (build 1.6.0_03-b05, mixed mode)
marcelo@saturno:~/src/frostwire-4.13.4.noarch$
```

Ahora lo que hay que hacer es modificar la variable PATH, para que cuando uno escriba "java", Linux vaya a buscar la versión de 32 bits:

```
marcelo@saturno:~/src/frostwire-4.13.4.noarch$ export PATH=/usr/lib/jvm/ia32-java-6-sun/bin:$PATH
marcelo@saturno:~/src/frostwire-4.13.4.noarch$
```

Ahora ejecutá de nuevo "java -version" (siempre en la misma terminal, ya que es el "ámbito" donde el cambio de PATH tiene efecto):

```
marcelo@saturno:~/src/frostwire-4.13.4.noarch$ java -version
java version "1.6.0_03"
Java(TM) SE Runtime Environment (build 1.6.0_03-b05)
Java HotSpot(TM) Client VM (build 1.6.0_03-b05, mixed mode, sharing)
marcelo@saturno:~/src/frostwire-4.13.4.noarch$
```

Joya, fijate que no dice "64 bits". :-)

Ahora todo lo que tenés que hacer es, en la misma ventana/pestaña de terminal donde hiciste el cambio de PATH, posicionarte en donde descomprimiste el .tar.gz (si ya no lo estabas):

```
marcelo@saturno:~$ cd home/marcelo/src/frostwire-4.13.4.noarch
marcelo@saturno:~/src/frostwire-4.13.4.noarch$
```

(yo lo descomprimí en "/home/marcelo/src/frostwire-4.13.4.noarch")

y ejecutá "./runFrostwire.sh". Listo, ya tenés andando frostwire. :-)

Para hacer esto más directo, podés abrir el archivo "runFrostwire.sh" con un editor de textos como el GEdit, y en la línea 11, abajo o arriba del "export HOSTNAME=localhost", podés poner esta línea:

```
export PATH="/usr/lib/jvm/ia32-java-6-sun/bin:$PATH"
```

Listo, ya te anda "siempre", sólo te queda hacer el enlace desde el editor de menú de Ubuntu o creando el lanzador de la aplicación en el escritorio.

Bueno, espero que les sirva y que quede en Google por si alguien más tiene el mismo problema...

Saludos!
Marcelo
