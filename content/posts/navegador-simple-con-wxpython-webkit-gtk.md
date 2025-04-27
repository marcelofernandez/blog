---
author: mfernandez
category:
  - codear
  - linux
  - programación
  - python
  - ubuntu-ar
date: "2010-10-10T01:28:54+00:00"
guid: https://blog.marcelofernandez.info/?p=995
title: Navegador simple con wxPython + Webkit/GTK
url: /2010/10/navegador-simple-con-wxpython-webkitgtk/

---
Hace algunos posts (¡casi un año ya!) [escribí](/2009/11/navegador-simple-con-pywebkitgtk/) sobre una manera fácil y rápida de tener un componente "navegador web" en Python sobre Linux/BSD, gracias a [PyGTK](http://www.pygtk.org) y [WebkitGTK](http://webkitgtk.org/), llamado lógicamente, [pyWebkitGTK](http://code.google.com/p/pywebkitgtk/). En pocas líneas de código uno puede disponer de un navegador potente y completo en un panel de su aplicación basada en [PyGTK](http://www.pygtk.org), ideal para integrar aún más la cada omnipresente Web.

Las vueltas de la vida y las ganas de experimentar y aprender te llevan a probar otros frameworks/librerías, como lo es [wxPython](http://www.wxpython.org); tanto es así que de vez en cuando tengo el placer de dar [alguna charla](/charlas/) al respecto [\[1\]](#nota1), y una de las debilidades que le usualmente le encontraba es la falta de un componente "browser web" nativo y soportado en todas las plataformas (wxPython sólo incluye IE embebible como ActiveX en Windows).

En búsqueda de alternativas existe [wxWebKit](http://wxwebkit.wxcommunity.com/), pero al proyecto le faltan terminar algunas cosas para tener lista su versión "1.0", y si bien [wxWebConnect](http://www.kirix.com/labs/wxwebconnect/) funciona, no da soporte para wxPython, sólo wxWidgets desde C++. Eso nos deja con que en Linux/BSD no tenemos componente que nos dé esta posibildad, pero... si wxPython en estas plataformas utiliza GTK por debajo, ¿no podríamos usar pyWebkitGTK como componente para embeberlo en nuestra aplicación Python?

La respuesta por suerte es afirmativa, y en una rápida búsqueda en la [Wiki de wxPython](http://wiki.wxpython.org) encontré [un ejemplo de cómo hacerlo](http://wiki.wxpython.org/wxGTKWebKit). Si bien podría copiar y pegar la receta, me gustaría " _aggiornarla_" un poquito, al menos traduciendo y explicando un poco más los comentarios.

Primero vamos a mostrar cómo se vería el módulo que incluye un widget HtmlPanel, wxwebkitgtk.py:

```
#!/usr/bin/env python
# coding:utf-8

"""
    wxWebkitGTK - Componente wxPython que embebe un navegador
                   utiliza la biblioteca Webkit GTK desde Python (PyWebkitGTK).

    Marcelo Fidel Fernández - http://www.marcelofernandez.info
    Basado en: http://wiki.wxpython.org/wxGTKWebKit
"""
import os
import wx
import gobject
gobject.threads_init()
import gtk, gtk.gdk
import webkit

class HtmlPanel(wx.Panel):

    def __init__(self, *args, **kwargs):
        wx.Panel.__init__(self, *args, **kwargs)
        # Aquí es donde se hace la "magia" de embeber webkit en wxGTK.
        whdl = self.GetHandle()
        window = gtk.gdk.window_lookup(whdl)
        # Debemos mantener la referencia a "pizza", sino obtenemos un segfault.
        self.pizza = window.get_user_data()
        # Obtengo el padre de la clase GtkPizza, un gtk.ScrolledWindow
        self.scrolled_window = self.pizza.parent
        # Saco el objeto GtkPizza para poner un WebView en su lugar
        self.scrolled_window.remove(self.pizza)
        self.webview = webkit.WebView()
        self.scrolled_window.add(self.webview)
        self.scrolled_window.show_all()
```

La "magia" consiste en que, sabiendo que pyWebkitGTK necesita un componente [ScrolledWindow](http://library.gnome.org/devel/pygtk/stable/class-gtkscrolledwindow.html) GTK como padre para funcionar correctamente, se utiliza la biblioteca PyGTK para buscar el ScrolledWindow GTK donde está embebido el wx.Panel de la clase (su "abuelo"), y reemplazar el hijo GTKPizza (un componente inventado por wxWidgets para funcionar) por el WebView de Webkit.

Aquí hay un ejemplo de cómo se puede utilizar este panel como widget de wxPython completo e independiente, copiando la funcionalidad básica del [post de PyWebkitGTK](/2009/11/navegador-simple-con-pywebkitgtk/), el archivo wxwebkitgtk\_demo.py:

```
#!/usr/bin/env python
# coding: utf-8

"""
    wxSimpleBrowser - Navegador muy muy simple de internet, sólo de ejemplo,
                      que utiliza la biblioteca Webkit GTK desde wxPython.

    Marcelo Fidel Fernández - http://www.marcelofernandez.info
    Licencia: BSD. Disponible en: http://www.freebsd.org/copyright/license.html
"""

import sys
import wx
from wxwebkitgtk import HtmlPanel

DEFAULT_URL = 'http://www.python.org'

class wxSimpleBrowser(wx.Frame):

    def __init__(self):
        wx.Frame.__init__(self, None)
        self.TxtUrl = wx.TextCtrl(self, wx.ID_ANY, style=wx.TE_PROCESS_ENTER)
        self.TxtUrl.Bind(wx.EVT_TEXT_ENTER, self.OnTxtURL)
        self.Box = wx.BoxSizer(wx.VERTICAL)
        self.Box.Add(self.TxtUrl, proportion=0, flag=wx.EXPAND)
        self.SetSizer(self.Box)
        self.SetSize((800,600))
        self.Show()
        # Necesitamos tener mostrado el componente padre del Panel para que funcione,
        # por eso mostramos primero el Frame y después creamos el HtmlPanel
        self.HtmlPanel = HtmlPanel(self)
        self.Box.Add(self.HtmlPanel, proportion=1, flag=wx.EXPAND)
        self.SendSizeEvent() # Para acomodar el panel al tamaño del frame

    def OnTxtURL(self, event):
        self.Open(self.TxtUrl.GetValue())

    def Open(self, url):
        # Podemos acceder a todos los métods del objeto WebView
        # http://webkitgtk.org/reference/webkitgtk-webkitwebview.html
        self.HtmlPanel.webview.load_uri(url)
        self.TxtUrl.SetValue(url)
        self.SetTitle('wxSimpleBrowser - %s' % url)

if __name__ == '__main__':
    if len(sys.argv) > 1:
        url = sys.argv[1]
    else:
        url = DEFAULT_URL
    app = wx.App()
    browser = wxSimpleBrowser()
    browser.Open(url)
    app.MainLoop()
```

Creo que para la enorme funcionalidad que nos brinda el proceso de ponerlo en práctica es bastante simple, y aunque depende de PyGTK, ésta biblioteca hoy está disponible "de fábrica" en cualquier distribución moderna de GNU/Linux.

De aquí en más es ser muy sencillo dejar al lector el armado de un widget para wxPython que en Windows muestre el componente navegador de IE y en Linux un navegador Webkit.

\[1\] ¡La semana que viene voy a estar en la [PyCon Argentina 2010](http://ar.pycon.org/2010/about/) dando una charla de [Introducción a wxPython](http://ar.pycon.org/2010/conference/schedule/event/55/)! :-D

¡Saludos!
