---
author: Marcelo
category:
  - codear
  - linux
  - ubuntu-ar
date: "2006-12-07T13:15:00+00:00"
guid: https://blog.marcelofernandez.info/?p=33
title: Tipeando Caracteres Especiales en Gnome
url: /2006/12/tipeando-caracteres-especiales-en-gnome/

---
Hola muchachios. Ultimamente no tuve nada importante que bloguear, así que para no hacer sapo esta semana, les regalo un mail que escribí a mediados del 2006 a la lista del [UnluX](http://www.unlux.com.ar/).

Si bien son algo viejos, del grueso listado de hacks que hay para gnome \[1\], me
gustó encontrar este:

[http://gnome-hacks.jodrell.net/hacks.html?id=76](http://gnome-hacks.jodrell.net/hacks.html?id=76)

Que alegremente cuenta cómo ingresar caracteres ésos Unicode que no
encontramos en el teclado (saber que Alt+64 en Windows sirve para tipear
la arroba es la primera pregunta que sabe responder todo dueño de
cibercafé) :-D

Sencillamente es cuestión de teclear: Control+Shift+Código\_Unicode
Es decir, Control+Shift+0040 = @

Tabla útil para la cartera de la dama y el bolsillo del caballero:

```
Control+Shift+0040 = @
Control+Shift+00E1 = á
Control+Shift+00E9 = é
Control+Shift+00ED = í
Control+Shift+00F3 = ó
Control+Shift+00FA = ú
Control+Shift+00B0 = °
Control+Shift+00BA = º
Control+Shift+00F1 = ñ
Control+Shift+00D1 = Ñ
Control+Shift+00FC = ü
```

Esto obviamente surge porque está mal configurado el teclado, pero
siempre alguno que migra de Windows pregunta por qué no funciona
Alt+64... acá tá la respuesta).

Particularidades en mi Ubuntu (codificación es\_AR.UTF8):
\- No funca con aplicaciones QT-KDE.
\- No funca en la consola (la de texto).
\- Sí funca en todas las aplicaciones GTK-Gnome, incluído el Terminal.
\- Funca de la misma manera tipeando los últimos dos caracteres
relevantes (nos ahorramos de tipear "00" todo el tiempo). Esto
obviamente no nos va a dejar escribir caracteres a nuestros primos
japoneses o árabes, pero sí a mi tía Blanca que vive en Bahía Blanca. :-D

2da. versión de la tablita (para vagos como yo):

```
Control+Shift+40 = @
Control+Shift+E1 = á
Control+Shift+E9 = é
Control+Shift+ED = í
Control+Shift+F3 = ó
Control+Shift+FA = ú
Control+Shift+B0 = °
Control+Shift+BA = º
Control+Shift+F1 = ñ
Control+Shift+D1 = Ñ
Control+Shift+FC = ü
```

Link directo al listado de hacks de Gnome (no lo leí completo, pero
parece pulenta)
\[1\] [http://gnome-hacks.jodrell.net/all.html](http://gnome-hacks.jodrell.net/all.html)

La Yapa:
[http://live.gnome.org/PowerUserTools](http://live.gnome.org/PowerUserTools)

Salutes Gnomeros! (Yo digo "jenomeros", pero cada uno que lo adapte a su
religión). :-P
Marcelo
PD: Extraído de mi-mismo-hace-mucho-tiempo en [http://linux.org.ar/pipermail/unlug/2006-July/000875.html](http://linux.org.ar/pipermail/unlug/2006-July/000875.html)
PD2: Gracias [codeAR](http://www.codear.com.ar) por linkearme!
