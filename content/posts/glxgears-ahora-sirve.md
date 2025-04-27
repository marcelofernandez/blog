---
author: mfernandez
category:
  - codear
  - linux
date: "2006-11-21T20:28:00+00:00"
guid: https://blog.marcelofernandez.info/?p=28
title: Glxgears ahora sirve!
url: /2006/11/glxgears-ahora-sirve/

---
Una huevada que hace tiempo que no sabía qué lo estaba causando (sólo en los Ubuntu), era que el glxgears no me escupía por consola los Frames por Segundo (FPS) que lograba renderizar en 3D. Antes yo podía ver que si tiraba unos 40 o 50 FPS, algo con mi 3D andaba mal... cuando cambiaba de placa de video, veía cúantos FPS tiraba el glxgears y comparaba la diferencia relativa de placas... etc.

Hasta que migré a Ubuntu, donde los FPS nunca aparecen en la consola!!
Siendo hasta una cuestión existencial, busqué y encontré la solución: como parece que a alguien le molestó que el glxgears se utilizara como "benchmark", por defecto se deshabilitó la salida de los benditos FPS (mal hecho en mi opinión).

Por suerte, dejaron una "puerta trasera", una opción de consola. Tipee esto en su consola:

```
marcelo@marcelo-desktop:~$ glxgears -iacknowledgethatthistoolisnotabenchmark
```

Y obtenemos esto!

[![](http://photos1.blogger.com/blogger2/448/981953459584652/400/Pantallazo.jpg)](http://photos1.blogger.com/blogger2/448/981953459584652/1600/Pantallazo.jpg)

Conclusiones:
1- Lo bueno es que ahora sé a cuánto anda más o menos mi placa con X.org 7.1 + Drivers Nvidia 9xxx sobre Beryl.
2- A QUIEN SE LE PUEDE HABER OCURRIDO SEMEJANTE OPCION???? ("yo reconozco que esta herramienta no es un bechmark"??????????)

Y bueh, es lo que hay.
Salutes
Marcelo

Actualización: [Mauro](http://www.blogger.com/profile/01278891690753446135) me comenta de que el flag '-printfps' debería funcionar y así es, funciona. Así que hay 2 flags para hacer lo mismo, el 'fácil' y el 'geek'. :-P
