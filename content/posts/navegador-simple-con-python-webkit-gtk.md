---
author: mfernandez
category:
  - codear
  - programación
  - python
  - ubuntu-ar
date: "2009-11-19T21:56:12+00:00"
guid: https://blog.marcelofernandez.info/?p=501
title: Navegador simple con Python + Webkit/GTK
url: /2009/11/navegador-simple-con-pywebkitgtk/

---
Hoy me encontré con otro [un hilo en la lista de PyAr](http://mx.grulic.org.ar/lurker/message/20091118.001651.3f9b4974.es.html) que me deja un link más que interesante: ¡Existe un binding para usar [Webkit sobre GTK desde Python](http://code.google.com/p/pywebkitgtk/), y lo mejor de todo es que [ya está incluido en los repositorios](http://packages.ubuntu.com/karmic/python-webkit) de Ubuntu 9.10!

[Webkit](http://www.webkit.org) es un motor de [renderizado](http://es.wikipedia.org/wiki/Renderización) ("dibujado") de páginas web, que es utilizado en el corazón en cada vez más navegadores, como [Chrome](http://www.google.com.ar/chrome), [Safari](http://www.apple.com/es/safari/), [Konqueror](http://www.konqueror.org/), etc. Es super completo y veloz; y permite ejecutarse en muchísimas plataformas y sistemas diferentes.  Si bien existen otros métodos para embeber un navegador en una aplicación PyGTK, como por ejemplo [gtkmozembed](http://www.pygtk.org/pygtkmozembed/class-gtkmozembed.html) (que embebe el motor de [Firefox](http://www.mozilla-europe.org/es/firefox/)), éste no es muy poderoso, o por lo menos no deja meterle mucha "mano" para personalizarlo, y uno termina teniendo relativamente muy poco "poder". En cambio con Webkit/GTK se pueden hacer muchas más cosas, tan sólo hace falta ver la documentación y un ejemplo (links al final, claro). :-)

No podía dejar de probarlo. Entonces me puse manos a la obra, y salió esto, tratando de imitar lo que se posteó en la lista:

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
    SimpleBrowser - Navegador muy muy simple de internet, sólo de ejemplo,
                    que utiliza la biblioteca Webkit GTK desde Python (PyWebkitGTK).

    Marcelo Fidel Fernández - http://www.marcelofernandez.info
    Licencia: BSD. Disponible en: http://www.freebsd.org/copyright/license.html
"""

import sys
import gtk
import webkit

DEFAULT_URL = 'http://www.python.org'

class SimpleBrowser:

    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.set_position(gtk.WIN_POS_CENTER_ALWAYS)
        self.window.connect('delete_event', self.close_application)
        self.window.set_default_size(800, 600)

        vbox = gtk.VBox(spacing=5)
        vbox.set_border_width(5)

        self.txt_url = gtk.Entry()
        self.txt_url.connect('activate', self._txt_url_activate)

        self.scrolled_window = gtk.ScrolledWindow()
        self.webview = webkit.WebView()
        self.scrolled_window.add(self.webview)

        vbox.pack_start(self.txt_url, fill=False, expand=False)
        vbox.pack_start(self.scrolled_window, fill=True, expand=True)
        self.window.add(vbox)

    def _txt_url_activate(self, entry):
        self._load(entry.get_text())

    def _load(self, url):
        self.webview.open(url)

    def open(self, url):
        self.txt_url.set_text(url)
        self.window.set_title('SimpleBrowser - %s' % url)
        self._load(url)

    def show(self):
        self.window.show_all()

    def close_application(self, widget, event, data=None):
        gtk.main_quit()

if __name__ == '__main__':
    if len(sys.argv) > 1:
        url = sys.argv[1]
    else:
        url = DEFAULT_URL

    # PyWebkitGTK necesita habilitar el soporte de los hilos en PyGTK
    gtk.gdk.threads_init()
    browser = SimpleBrowser()
    browser.open(url)
    browser.show()
    gtk.main()
```

¡Y Listo!

 [![Pantallazo_PyWebkitGTK](/wp-content/uploads/2009/11/Pantallazo-300x233.png)](/wp-content/uploads/2009/11/Pantallazo.png)

Todo lo que se necesita en Ubuntu 9.10 para poder correr esto es instalar el paquete "python-webkit"; sin embargo, esta versión es la 1.1.5, mientras que PyWebkitGTK va por la 1.1.7 y Webkit/GTK va por la 1.1.15, así que todavía hay lugar para mejoras.

Aquí dejo algunos links:

- [PyWebkitGTK](http://code.google.com/p/pywebkitgtk/) se llama el proyecto de llevar [Webkit/GTK](http://webkitgtk.org/) (escrito naturalmente en C) a Python.
- [Acá hay un ejemplo](http://code.google.com/p/pywebkitgtk/source/browse/trunk/demos/browser.py) muchísimo más completo de un navegador con múltiples pestañas y todo.
- Lamentablemente, la documentación de la biblioteca en Python no existe aún (es un ticket del proyecto), así que por ahora habrá que conformarse con [la documentación de Webkit/GTK](http://webkitgtk.org/reference/index.html); sin embargo, yo lo encuentro bastante legible, ya que enseguida uno se adapta a "traducir" cómo se llamaría un método en C a uno en Python.

Espero que les sirva. Me divertí mucho haciéndolo. :-)

Saludos
