---
author: Marcelo
category:
  - codear
  - linux
  - programación
  - python
  - ubuntu-ar
date: "2010-04-15T18:04:29+00:00"
guid: https://blog.marcelofernandez.info/?p=663
title: Renderizando PDFs en Python con Poppler II
url: /2010/04/renderizando-pdfs-en-python-con-poppler-ii/

---
Hace unos días me llegó un mail de alguien preguntándome cómo, a partir de la [parte I de este artículo](/2009/05/renderizando-pdfs-en-python-con-poppler/), hacer un sencillo visor de PDFs con [wxPython](http://www.wxpython.org). Me encontré con algunas dificultades, principalmente que el [ScrolledWindow](https://docs.wxpython.org/wx.ScrolledWindow.html) de wxPython no permite actualizarse dinámicamente, o automáticamente según el contenido (esto [sí es bastante sencillo en GTK](http://www.pygtk.org/docs/pygtk/class-gtkadjustment.html)); con lo cual se complicaba hacer zoom, modificar el tamaño de la ventana y adaptar los scrollbars, etc.

Sin embargo, con alguna vuelta de más pude armar un ejemplo, que paso a dejar [acá](http://code.activestate.com/recipes/577195-wxpython-pdf-viewer-using-poppler/):

```
#!/usr/bin/env python
# coding: utf-8

"""
    wxPDFViewer - Simple PDF Viewer using Python-Poppler and wxPython
    Marcelo Fidel Fernandez - MIT License
    http://www.marcelofernandez.info - marcelo.fidel.fernandez@gmail.com
"""

import wx
import wx.lib.wxcairo as wxcairo
import sys
import poppler

class PDFWindow(wx.ScrolledWindow):
    """ This example class implements a PDF Viewer Window, handling Zoom and Scrolling """

    MAX_SCALE = 2
    MIN_SCALE = 1
    SCROLLBAR_UNITS = 20  # pixels per scrollbar unit

    def __init__(self, parent):
        wx.ScrolledWindow.__init__(self, parent, wx.ID_ANY)
        # Wrap a panel inside
        self.panel = wx.Panel(self)
        # Initialize variables
        self.n_page = 0
        self.scale = 1
        self.document = None
        self.n_pages = None
        self.current_page = None
        self.width = None
        self.height = None
        # Connect panel events
        self.panel.Bind(wx.EVT_PAINT, self.OnPaint)
        self.panel.Bind(wx.EVT_KEY_DOWN, self.OnKeyDown)
        self.panel.Bind(wx.EVT_LEFT_DOWN, self.OnLeftDown)
        self.panel.Bind(wx.EVT_RIGHT_DOWN, self.OnRightDown)

    def LoadDocument(self, file):
        self.document = poppler.document_new_from_file("file://" + file, None)
        self.n_pages = self.document.get_n_pages()
        self.current_page = self.document.get_page(self.n_page)
        self.width, self.height = self.current_page.get_size()
        self._UpdateSize()

    def OnPaint(self, event):
        dc = wx.PaintDC(self.panel)
        cr = wxcairo.ContextFromDC(dc)
        cr.set_source_rgb(1, 1, 1)  # White background
        if self.scale != 1:
            cr.scale(self.scale, self.scale)
        cr.rectangle(0, 0, self.width, self.height)
        cr.fill()
        self.current_page.render(cr)

    def OnLeftDown(self, event):
        self._UpdateScale(self.scale + 0.2)

    def OnRightDown(self, event):
        self._UpdateScale(self.scale - 0.2)

    def _UpdateScale(self, new_scale):
        if new_scale >= PDFWindow.MIN_SCALE and new_scale <= PDFWindow.MAX_SCALE:
            self.scale = new_scale
            # Obtain the current scroll position
            prev_position = self.GetViewStart()
            # Scroll to the beginning because I'm going to redraw all the panel
            self.Scroll(0, 0)
            # Redraw (calls OnPaint and such)
            self.Refresh()
            # Update panel Size and scrollbar config
            self._UpdateSize()
            # Get to the previous scroll position
            self.Scroll(prev_position[0], prev_position[1])

    def _UpdateSize(self):
        u = PDFWindow.SCROLLBAR_UNITS
        self.panel.SetSize((self.width*self.scale, self.height*self.scale))
        self.SetScrollbars(u, u, (self.width*self.scale)/u, (self.height*self.scale)/u)

    def OnKeyDown(self, event):
        update = True
        # More keycodes in http://docs.wxwidgets.org/stable/wx_keycodes.html#keycodes
        keycode = event.GetKeyCode()
        if keycode in (wx.WXK_PAGEDOWN, wx.WXK_SPACE):
            next_page = self.n_page + 1
        elif keycode == wx.WXK_PAGEUP:
            next_page = self.n_page - 1
        else:
            update = False
        if update and (next_page >= 0) and (next_page < self.n_pages):
                self.n_page = next_page
                self.current_page = self.document.get_page(next_page)
                self.Refresh()

class MyFrame(wx.Frame):

    def __init__(self):
        wx.Frame.__init__(self, None, -1, "wxPdf Viewer", size=(800,600))
        self.pdfwindow = PDFWindow(self)
        self.pdfwindow.LoadDocument(sys.argv[1])
        self.pdfwindow.SetFocus() # To capture keyboard events

if __name__=="__main__":
    app = wx.App()
    f = MyFrame()
    f.Show()
    app.MainLoop()
```

Si bien tengo entendido que el scroll no es "óptimo", ya que en cada OnPaint() se redibuja todo el PDF, anda bastante bien en mi instalación y no parece ser un problema. Lógicamente sí se puede redibujar un área en particular con Cairo (sería la que queda "invalidada" por el scroll), pero wxPython me explotó al intentar algunas cosas por un lado, y me parece que en realidad no sé hacerlo bien por el otro (si alguien puede tirar una soga en esto en particular, bienvenido sea).

Me entretuve mucho haciendo esto, y espero que a alguien le sea de utilidad; y aunque todavía Python-Poppler no está disponible para Windows en forma binaria (pero puede ser compilado, [bug acá](https://bugs.launchpad.net/poppler-python/+bug/499592)), sí se puede usar en cualquier Linux, OSX, etc. Y nuevamente, hay una [demo](http://bazaar.launchpad.net/~poppler-python/poppler-python/trunk/annotate/head:/demo/demo-poppler.py) para hacer esto mismo con PyGTK, y la [demo de Poppler en C](http://cgit.freedesktop.org/poppler/poppler/tree/glib/demo) es mucho más completa, además de fácil de leer y traducir a Python.

Saludos
