---
author: mfernandez
category:
  - codear
  - programación
date: "2006-12-18T12:54:00+00:00"
guid: https://blog.marcelofernandez.info/?p=35
title: SWT ahora se porta como un .jar
url: /2006/12/swt-ahora-se-porta-como-un-jar/

---
Hace un tiempo, basado en la experiencia que tuve y tengo haciendo soft con GUIs, al utilizar [Java](http://java.sun.com/) se me plantearon las dos alternativas serias que existen en el lenguaje: [Swing](http://en.wikipedia.org/wiki/Java_Swing) y [SWT](http://www.eclipse.org/swt/).

Si bien el tema es motivo de " [guerra santa](http://es.wikipedia.org/wiki/Flamer)" permanente entre la gente que está a favor de [Sun](http://www.sun.com/), [Netbeans](http://www.netbeans.org/) y por último, Swing contra la de [IBM](http://www.ibm.com/), [Eclipse](http://www.eclipse.org/) y [SWT](http://en.wikipedia.org/wiki/Standard_Widget_Toolkit), yo probé un poquito de las dos. Me gustó más SWT, más que nada porque Swing se veía horrible y lento en Linux (y no me parece cambiar de SO por ese motivo). Así que, como el que tenía la decisión era yo, hice entre otras cosas chiquitas, un programita para enviar SMS a través de internet a los celulares de Argentina, [Sonar](http://smsar.sourceforge.net/).

[Sonar](http://smsar.sourceforge.net/galeria/index.html) está escrito en Java+SWT, y como éste último utiliza los widgets nativos de la plataforma, existe un swt.jar para interfacear desde java (lo que se llama un [wrapper](http://en.wikipedia.org/wiki/Wrapper_pattern)), que llama a las distintas librerías compartidas nativas, según la plataforma donde esté corriendo.

Algo así, comparado con Swing:

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/RYLpqaCIQvI/AAAAAAAAAAY/wACprq6EhjQ/s400/figure_03.gif)](http://2.bp.blogspot.com/_nDZ247g0qSM/RYLpqaCIQvI/AAAAAAAAAAY/wACprq6EhjQ/s1600-h/figure_03.gif)(Extraído de [http://www.developer.com/open/article.php/10930\_3316241\_2](http://www.developer.com/open/article.php/10930_3316241_2))

No quiero seguir con esta intro, para eso está Google, la Wikipedia y [éste link](http://www.informit.com/guides/content.asp?g=java&seqNum=86&rl=1). :-D

Como SWT utiliza librerías nativas, cada vez que se quiera ejecutar un programa que las utilice, debe ejecutarse configurando en el java.library.path, la ubicación de las librerías nativas (excepto todo lo que pertenezca al [JDK](http://en.wikipedia.org/wiki/JDK) en sí). Y SWT no está en la JDK.

Por ejemplo, este script [Bash](http://es.wikipedia.org/wiki/Bash) de inicio es típico de un programa en SWT:

```
#!/bin/bash
export PATH=$PATH:$(dirname $(readlink -f $0))
java -Djava.library.path=./swt/ -jar sonar.jar
```

(El [mecanismo de MANIFEST.MF](http://en.wikipedia.org/wiki/JAR_%28file_format%29) que incluye el .jar incluye por su parte a ./swt/swt.jar y listo.) [![](http://2.bp.blogspot.com/_nDZ247g0qSM/RYLytaCIQ0I/AAAAAAAAABE/n4G6_UFRY6A/s320/lin-example.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/RYLytaCIQ0I/AAAAAAAAABE/n4G6_UFRY6A/s1600-h/lin-example.png)
Esto era un inconveniente, más que nada para los usuarios de Swing, que no estaban acostumbrados a esto, ya que Swing está dentro de la JDK. Mecanismos como [Java Web Start](http://en.wikipedia.org/wiki/Java_web_start) (que me parecen muy útiles) eran complicados de implementar con SWT, siendo éste el principal motivo.

El punto es que una buena noticia de la última versión de desarrollo de SWT (la 3.3M4) va a incluir las librerías nativas en el mismo swt.jar, haciendo innecesario definir esta constante.

```

```

Esto permite que todos los mecanismos estándar de ejecución de aplicaciones (hacer "doble click" en el .jar en Windows y Linux, por ejemplo) se apliquen a SWT tal como se aplican a Swing.

SWT, en mi opinión, es uno de los mejores toolkits multiplataforma (que utilizan el look and feel nativo) que conozco. Sólo con Java está bueno. Combinado con [Jython](http://beta.blogger.com/www.jython.org) es una masa!

Más info de la noticia:

- [New and Noteworthy de SWT 3.3M4](http://www.eclipse.org/swt/R3_3/new_and_noteworthy.html#m4)
- [EclipseZone](http://www.eclipsezone.com/eclipse/forums/t86175.rhtml)

Saludos!
Marcelo
