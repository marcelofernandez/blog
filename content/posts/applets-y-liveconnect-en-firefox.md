---
author: mfernandez
category:
  - codear
  - programación
  - web
date: "2006-11-24T12:13:00+00:00"
guid: https://blog.marcelofernandez.info/?p=30
title: Applets y LiveConnect en Firefox
url: /2006/11/applets-y-liveconnect-en-firefox/

---
Holas. Ultimamente estuve metiendo mano en un [Applet de Java](http://en.wikipedia.org/wiki/Java_applet) embebido en un Sistema Web, que entre otras cosas, lo que hacía era ejecutar lo siguiente:

```
import java.applet.Applet;
import netscape.javascript.JSException;
import netscape.javascript.JSObject;

public class imprimesecescape01 extends Applet {
  public void paint(Graphics g) {
      // .... código Java haciendo lo que hace el Applet ...
      // Al finalizar:
      try {
          JSObject jsobject = JSObject.getWindow(this);

          // windowClose es una función Javascript que
          // está en el HTML que sólo llama a window.close();
          jsobject.call("windowClose", null);

      } catch(JSException jsexception) {
          g.drawString("caught JS: " + jsexception, 20, 20);
      }
  }
}
```

Es decir, este Applet utiliza las clases de [LiveConnect](http://en.wikipedia.org/wiki/LiveConnect) del navegador para llamar a la función [JavaScript](http://en.wikipedia.org/wiki/Javascript) windowClose() residente en el HTML, que a su vez llama a window.close(). Todo para que, una vez que se ejecutó el código del applet, se cierre la ventana.

Esto en IE (previamente el único navegador soportado por el sistema) funciona. Pero en Firefox no. Después de investigar, tenía un problema y un bug en mi applet:

1- Un bug en el applet: El applet no se ejecutaba a menos que el tag APPLET especificara algo mayor a 0 en el width y el height. Encontré [este bug](https://bugzilla.mozilla.org/show_bug.cgi?id=240314), pero aparece como RESOLVED. Al principio, pensé que había una regresión del bug. Revisando y pensando un poquito, el comportamiento que mostraba Firefox estaba bien, mientras que el de IE estaba mal.

Por qué? Simple:
Pregunta: cuándo se ejecuta el método paint()?
Respuesta: cuando hay que dibujar el applet en su área asignada en la página.

Entonces, si el área a dibujar es de 0 pixeles de ancho y 0 de alto, para qué ejecutar el método paint? Justamente, Firefox no debía dibujar nada, entonces no llamaba a paint()... mientras que IE lo ejecutaba igual... grrr..

Al final, fue tanto como mudar el ćodigo de paint() a start() (que se ejecuta únicamente cuando el navegador arranca el applet), y listo el pollo. :-D

2- Un problema: El código que está arriba (el del applet), que llama a windowClose(), colgaba el firefox! Encontré [este otro bug](https://bugzilla.mozilla.org/show_bug.cgi?query_format=specific&order=relevance+desc&bug_status=__open__&id=74482) (del 2001!), que tiene un workaround:

```
try {
  JSObject jsobject = JSObject.getWindow(this);
  jsobject.eval("setTimeout(\"windowClose()\",100)");
} catch(JSException jsexception) {
  g.drawString("caught JS: " + jsexception, 20, 20);
}
```

Si claro, es un hack, pero funciona tanto en Firefox como en Explorer, así que ahora mi applet funciona y el sistema ahora está "portado" a Firefox/Linux también! :-D

"Todo sea por un cacho más de libertad" :-P

Marcelo
