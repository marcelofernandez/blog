---
author: Marcelo
category:
  - codear
  - programación
  - python
  - sysadmin
  - ubuntu-ar
date: "2010-04-02T18:01:11+00:00"
guid: https://blog.marcelofernandez.info/?p=601
title: Reemplazando texto con expresiones regulares en Python
url: /2010/04/reemplazando-texto-con-expresiones-regulares-en-python/

---
Hay veces en que uno necesita automatizar tareas, como reemplazar cierto texto por otro bajo ciertas condiciones, y el viejo "%s/cosa/otra/g" [del vim](http://www.linux.com/learn/tutorials/8255-vim-tips-the-basics-of-search-and-replace) nos queda corto. En mi caso en particular, estaba metiendo algunas pequeñas características en [PyFpdf](http://www.sistemasagiles.com.ar/trac/wiki/PyFpdf), y vi que había [algunos archivos .py](http://www.sistemasagiles.com.ar/trac/browser/pyfpdf/font/times.py?rev=103) llenos de llamadas a la función [chr()](http://docs.python.org/library/functions.html#chr).

Claro, PyFpdf es un port más o menos "haragán" ( _lazy_) de [Fpdf para PHP](http://www.fpdf.org/), y el autor original evidentemente encontró más sencillo definir algunas fuentes (en binario) haciendo sucesivas llamadas a la función chr(), como  esta:

```
fpdf_charwidths['times']={chr(0):250, chr(1):250, chr(2):250, chr(3):250,} # Sigue...
```

Enseguida pensé en mejorarlo, reemplazando esas sucesivas llamadas a chr() por la misma función pero ya evaluada y en forma de byte string; por ejemplo, la idea era convertir lo anterior en:

```
fpdf_charwidths['times']={'\x00':250,'\x01':250,'\x02':250,'\x03':250,} # Sigue...
```

De esa manera, los archivos .py se simplificarían (se harían más legibles), no sufrirían modificaciones en su comportamiento y hasta serían más veloces en su interpretación y ejecución.

Primero se me ocurrió primero hacer algo más "a mano", pero en cuanto se me complicó un poquito enseguida pensé que la mejor herramienta era usar [expresiones regulares](http://es.wikipedia.org/wiki/Expresiones_regulares) para el matching del patrón "chr(x)" y, por consiguiente, el [módulo re](http://docs.python.org/library/re.html) de la librería estándar de Python.

Leyendo [esta excelente página](http://www.amk.ca/python/howto/regex/) y la documentación del módulo en cuestión, armé un script, y me quedó así:

```
#!/usr/bin/python
# coding:utf-8

# This script tries to identify all chr(XX) constant calls in python scripts
# and replace them with '\xXX' strings.
# Author: Marcelo Fernández - License: MIT

import sys
import re

def chrrepl(match):
    # See http://www.amk.ca/python/howto/regex/regex.html
    # Use the captured group to get the hex string value.
    char_number = match.group(1)
    return repr(chr(int(char_number)))

if __name__ == '__main__':
    if len(sys.argv) != 3:
        print 'Usage: python chr_cleaner.py infile.py outfile.py'
        sys.exit(1)

    infile = sys.argv[1]
    outfile = sys.argv[2]
    # Open file for reading
    try:
        fin = open(infile, 'r')
        fout = open(outfile, 'w')
    except IOError:
        print 'Error when reading %s or trying to write %s' % (infile, outfile)
        sys.exit(2)

    intext = fin.read()
    pattern = 'chr\\((\\d+)\\)' # Group the chr() function parameter to capture it
    p = re.compile(pattern)
    outtext = p.sub(chrrepl, intext)
    fout.write(outtext)
    fout.flush()

    fin.close()
    fout.close()
```

Si bien creo que el _snippet_ es bastante legible, lo interesante es que con el módulo re se puede:

- **Identificar un patrón en un texto**: En este caso el patrón sería "chr(\\d)" (\\d porque busco un número allí).
- **Marcar grupos en ese patrón utilizarlos luego**; como sé que voy a utilizar el número en cuestión para convertirlo a su byte string, lo defino dentro de un grupo. El patrón queda entonces "chr((\\d))". También es posible usar _named groups_, identificables por nombre en vez de por posición, pero eso lo dejo de tarea al lector :-P
- **Reemplazar todo el texto que coincide con ese patrón, y en cada reemplazo, llamar a una función que especifique "lo que hay que poner" allí, devolviendo un string**. Esta es la frutilla del postre... :-)
  Si bien puedo usar p.sub() para reemplazar lo que busco por un texto constante, también puedo hacer que en cada ocurrencia del patrón una función cualquiera sea llamada, y  se le pase la instancia de [MatchObject](http://docs.python.org/library/re.html#re.MatchObject) para que decidamos qué hacer con lo encontrado y devolver un string. Esto hace la función chrrepl() del snippet. Allí tomo el primer grupo (el grupo 0 es el string completo, el 1 es el primer grupo definido posicionalmente, y así sucesivamente), lo convierto a entero, ejecuto la función chr() y devuelvo su representación en byte string.

En fin, después de experimentar un buen rato, esto funcionó y [acá se puede ver](http://www.sistemasagiles.com.ar/trac/changeset/104/pyfpdf/) el diff del commit correspondiente para que se aprecie un poco mejor...

Saludos
