---
author: mfernandez
category:
  - codear
  - linux
  - programación
  - python
date: "2009-05-02T21:34:00+00:00"
guid: https://blog.marcelofernandez.info/?p=131
title: Renderizando PDFs en Python con Poppler
url: /2009/05/renderizando-pdfs-en-python-con-poppler/

---
Buenas... hoy voy a mostrar una forma de renderizar archivos PDF dentro de nuestras aplicaciones y desde Python, para casi cualquier plataforma con la estemos trabajando.

Introducción
Si bien cuando se arma una aplicación de escritorio se suele "llamar" a un software externo ( [Acrobat Reader](http://www.adobe.com/es/products/reader/), [Fox-It](http://www.foxitsoftware.com/pdf/reader/), [Evince](http://projects.gnome.org/evince/), etc.) para que muestre el contenido de un archivo [PDF](http://es.wikipedia.org/wiki/Pdf), generalmente queda "feo"; nuestro programa pierde la interacción y el control de lo que el usuario hace con el mismo, implica superponer un programa arriba del otro, etc. Esto suele producir una experiencia dificultosa para el usuario de nuestro querido programa, y como corresponde, los programadores nos ponemos tristes. :-(

Como una alternativa tenemos, en plataformas Windows, y estando el Acrobat Reader instalado previamente, la posibilidad de embeber en nuestro programa el mismo como un control [ActiveX](http://es.wikipedia.org/wiki/ActiveX) (hay [otras alternativas](http://en.wikipedia.org/wiki/NPAPI)) pero en el resto de las plataformas este problema no tenía solución (que yo conociera, al menos), ni siquiera algo más "liviano". El resultado de embeber todo el Acrobat Reader es más o menos lo que hacen los navegadores web cuando tienen el plugin de Adobe instalado y hacemos click en un link a un archivo PDF de la web:

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/SfytT-tbDFI/AAAAAAAACRk/l7lD3MdUIZw/s400/pdf.jpg)](http://www.bloginformatico.com/adobe-acrobat-reader-manipula-documentos-en-pdf.php)(Después de 1 minuto, y si el usuario no cerró la ventana, aparece esto)

Hace unos días, en [la lista de usuarios](http://lists.wxwidgets.org/mailman/listinfo/wxpython-users) de las bibliotecas [wxPython](http://www.wxpython.org/) [preguntaron si se podía renderizar un PDF](http://aspn.activestate.com/ASPN/Mail/Message/wxpython-users/3708367) dentro de una ventana del mismo programa, pero sobre Linux, y buscando la solución me encontré con un binding [Python para Poppler](https://launchpad.net/poppler-python).

Solución: Poppler-Python sobre Cairo
Veamos, [Poppler](http://poppler.freedesktop.org/) es una biblioteca de código abierto desarrollada en C con el objetivo de [renderizar](http://es.wikipedia.org/wiki/Renderizaci%C3%B3n) (es decir, interpretar y dibujar) archivos PDF, siendo la evolución del viejo [Xpdf](http://www.foolabs.com/xpdf/). Es una pieza de software imprescindible en el escritorio de cualquier Sistema Operativo Libre, ya que es un trabajo bárbaro el que hace, y además es la piedra fundamental de programas como Evince y [Okular](http://okular.kde.org/) (más allá de que el PDF es un formato de archivo universal). Y lo mejor es que dibuja sobre cualquier [contexto de dibujo](http://cairographics.org/manual/cairo-context.html) de [Cairo](http://cairographics.org/), así que donde haya Cairo y compilador de C, puede estar Poppler\*.

Hasta acá, bien, Poppler se puede utilizar desde C (como lo hace Evince, el visor de PDFs de Gnome). Pero 'ete aquí que me encontré con [estos bindings](https://launchpad.net/poppler-python) de Poppler para Python, y si bien [el ejemplo que éstos incluyen](http://bazaar.launchpad.net/%7Epoppler-python/poppler-python/trunk/annotate/head%3A/demo/demo-poppler.py) utiliza [PyGTK](http://www.pygtk.org/), en realidad, sólo necesita de un Canvas Cairo (GTK dibuja mucho sobre Cairo y provee mecanismos para que el desarrollador lo utilice también); es el mismo Cairo [que incluye wxPython](http://www.wxpython.org/docs/api/wx.lib.wxcairo-module.html) a partir de la versión 2.8.9.0 (aunque se puede utilizar con [ctypes](http://python.net/crew/theller/ctypes/) [desde versiones anteriores a la 2.8.9](http://wiki.wxpython.org/UsingCairoWithWxPython)).

Un poco de Código
En fin, voy a tratar de explicarlo mejor, [esta](http://bazaar.launchpad.net/%7Epoppler-python/poppler-python/trunk/annotate/head%3A/demo/demo-poppler.py) es la manera de renderizar un PDF dentro de una aplicación PyGTK. Voy a pegar acá abajo lo relevante:

```
    def __init__(self):
        [...]
        self.document = poppler.document_new_from_file (uri, None)
        self.n_pages = self.document.get_n_pages()
        self.current_page = self.document.get_page(0)
        self.scale = 1
        self.width, self.height = self.current_page.get_size()
        [...]
        self.dwg = gtk.DrawingArea()
        self.dwg.set_size_request(int(self.width), int(self.height))
        self.dwg.connect("expose-event", self.on_expose)

[...]

    def on_expose(self, widget, event):
        cr = widget.window.cairo_create()
        cr.set_source_rgb(1, 1, 1)

        if self.scale != 1:
            cr.scale(self.scale, self.scale)

        cr.rectangle(0, 0, self.width, self.height)
        cr.fill()
        self.current_page.render(cr)
```

Como se puede ver, Poppler-Python se encarga de cargar el archivo, devolver la cantidad de páginas, etc., mientras que con GTK creamos el contexto de dibujo Cairo, y todo lo que hacemos es decirle a Poppler "dibujá acá, en este contexto Cairo".

Entonces me puse a jugar con wxPython para hacer lo mismo, !y funciona!

[Acá está](http://pastebin.com/fe17a865) el ejemplo:

```
#!/usr/bin/env python
import wx
import wx.lib.wxcairo
import sys
import poppler

class MyFrame(wx.Frame):

    def __init__(self):
        wx.Frame.__init__(self, None, -1, "Cairo Test", size=(500,400))
        self.Bind(wx.EVT_PAINT, self.OnPaint)
        uri = "file://" + sys.argv[1]
        self.document = poppler.document_new_from_file (uri, None)
        self.n_pages = self.document.get_n_pages()

        self.current_page = self.document.get_page(0)
        self.scale = 1
        self.width, self.height = self.current_page.get_size()
        self.SetSize((self.width, self.height))

    def OnPaint(self, event):
        dc = wx.PaintDC(self)
        cr = wx.lib.wxcairo.ContextFromDC(dc)
        cr.set_source_rgb(1, 1, 1)

        if self.scale != 1:
            cr.scale(self.scale, self.scale)

        cr.rectangle(0, 0, self.width, self.height)
        cr.fill()
        self.current_page.render(cr)

if __name__=="__main__":
    app = wx.App()
    f = MyFrame()
    f.Show()
    app.MainLoop()
```

Y acá está la captura de pantalla obligada, luego de ejecutar "python ejemplo.py /path/al/archivo.pdf" (click para agrandar):

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/Sfy6d-ogAiI/AAAAAAAACR0/tqDJFXFmug0/s400/pantallazog.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/Sfy6d-ogAiI/AAAAAAAACR0/tqDJFXFmug0/s1600-h/pantallazog.png) De esta manera, es posible utilizar Python, Poppler y wxPython (o PyGTK) para mostrar y manejar el contenido de un archivo PDF dentro de nuestra aplicación y en casi cualquier plataforma, sin recurrir a componentes de terceros. :-)

Consideraciones:
Bueno, quisiera comentar que poppler-python parece ser un proyecto "joven", ya que todavía sólo [está incluido](http://packages.ubuntu.com/jaunty/python-poppler) en los repositorios de Ubuntu 9.04 en adelante. Con lo cual, en caso de utilizarlo en otras distribuciones y/o plataformas, hay que hacer un [checkout desde sus repositorios](https://code.launchpad.net/%7Epoppler-python/poppler-python/trunk) y compilarlo; sus únicas dependencias son [pycairo](http://cairographics.org/pycairo/) y poppler (0.10.x o posterior). Es muy sencillo hacerlo (el típico ./configure, make y make install), y no tuve ningún problema, utilizando Ubuntu 8.10.

En caso de querer utilizar Poppler-Python sobre el Cairo que incluye wxPython, lo mejor es utilizar una versión 2.8.9.x o superior de wxPython (ya con eso es suficiente, porque incluye Cairo en el paquete). Ubuntu 9.04 trae wxPython 2.8.9.1, Poppler 0.10.x y Poppler-Python en los repositorios, así que con un único apt-get ya tenemos todo lo necesario para hacer andar el ejemplo de arriba.

En mi caso, para probarlo en Ubuntu 8.10, tuve que compilar poppler desde [los fuentes](http://poppler.freedesktop.org/poppler-0.10.6.tar.gz) (traía la versión 0.8.x) y después compilar python-poppler, por un lado. Por el otro, para utilizar Cairo sobre wxPython 2.8.8.0 (el que viene en Ubuntu 8.10), llamé a Cairo con ctypes, como dice [acá](http://wiki.wxpython.org/UsingCairoWithWxPython), y me quedó [este ejemplo](http://pastebin.com/f75683324). Este procedimiento sería similar para cualquier otra distribución con versiones viejas de Poppler y/o de wxPython. Para la distribución de nuestro software todo se simplifica, ya que se puede usar tranquilamente [Py2Exe](http://www.py2exe.org/), [cx\_Freeze](http://cx-freeze.sourceforge.net/) o [PyInstaller](http://pyinstaller.python-hosting.com/), por nombrar algunos. Pero eso es tema de otro post ;-)

\\* Poppler denomina Frontend a las diferentes APIs que tiene para ser utilizado, entre ellas QT4 (base de KDE) y Glib (GTK principalmente, base de Gnome). Y tiene como backend (es decir, necesita para dibujar) no sólo a Cairo, sino que también puede dibujar sobre Splash (ignoro qué es, y Google no me devuelve nada relevante).

Bueno, espero que al menos le sirva a alguien, que lo aprovechen y lo mejoren!

Saludos
Marcelo
