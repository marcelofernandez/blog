---
author: Marcelo
category:
  - codear
  - linux
  - ubuntu-ar
date: "2007-06-25T16:16:00+00:00"
guid: https://blog.marcelofernandez.info/?p=70
title: Tipeando Caracteres Unicode en Gnome (II)
url: /2007/06/tipeando-caracteres-unicode-en-gnome-ii/

---
Tratando de probar el "truquito" que [postié hace bastante](/2006/12/tipeando-caracteres-especiales-en-gnome/) pero ahora en Ubuntu Feisty, veo que si le doy al Ctrl+Alt+40 no me genera la @....

Buscando, buscando por qué, encontré esto:
[http://live.gnome.org/TwoPointFifteen/ReleaseNotes](http://live.gnome.org/TwoPointFifteen/ReleaseNotes)

que dice:
"Use CTRL-SHIFT-u instead of CTRL\_SHIFT- to generate unicode keys -- This is a change in behaviour so important to note ( [OlavVitters](http://live.gnome.org/OlavVitters))"

O sea, que a partir de Gnome 2.16 no es más Ctrl+Shift+Código\_Unicode, sino que es Ctrl+Shift+u+Código\_Unicode.

Vuelvo a postear el mismo listadito del post viejo, modificado. :-D

```
Control+Shift+u+40 = @
Control+Shift+u+E1 = á
Control+Shift+u+E9 = é
Control+Shift+u+ED = í
Control+Shift+u+F3 = ó
Control+Shift+u+FA = ú
Control+Shift+u+B0 = °
Control+Shift+u+BA = º
Control+Shift+u+F1 = ñ
Control+Shift+u+D1 = Ñ
Control+Shift+u+FC = ü
```

 [http://www.unicode.org/](http://www.unicode.org/)

Saludos
Marcelo
