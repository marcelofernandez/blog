---
author: mfernandez
category:
  - codear
  - programación
  - python
date: "2007-08-04T15:00:00+00:00"
guid: https://blog.marcelofernandez.info/?p=79
title: Metaprogramación en Python
url: /2007/08/metaprogramacion-en-python/

---
Python está bárbaro, es sencillo y todo... siempre leí que era muy fácil hacer metaprogramming con él, pero el problema es que uno haga metaprogramming de una forma fácil en su mente. :-P

Veamos, la función getattr(), disponible en el módulo \_\_builtins\_\_ (o sea, disponible en todo momento), me permite cambiar esto:

```
def get_transportistas_custodia(id_custodia):
  """ Devuelvo todos los transportistas que están relacionados con una custodia """
  c = Custodia.objects.get(id=1)
  # Necesito el conjunto de transportistas de la custodia
  return c.transportistacustodia_set.all()

def get_asegurados_custodia(id_custodia):
  """ Devuelvo todos los asegurados que están relacionados con una custodia """
...
```

(lo malo es que tengo que hacer una función por cada tipo de empresa relacionada con custodia).

Por esto:

```
def get_empresas_relacionadas_custodia(id_custodia, empresa_relacionada='transportista'):
  """ Devuelvo todos los objetos de la empresa relacionada que están
      relacionados con una custodia.
      empresa_relacionada es un string que contiene el tipo de empresa con la cual
      relacionar a custodia.
  """
  c = Custodia.objects.get(id_custodia)
  # Necesito el conjunto de empresas relacionadas de la custodia
  return getattr(c, empresa_relacionada + 'custodia_set').all()
```

Esto es muy bonito. Lo que no había podido hacer hasta ahora era parametrizar con una variable las llamadas a función. Hablo de cosas como esta:

```
def has_empresa_relacionada_custodia(id_custodia, empresa_relacionada='transportista',
                                      id_empresa):
  """ Devuelve true si la empresa + id de la empresa especificados están
      relacionados con el id_custodia """

  c = Custodia.objects.get(id_custodia)
  # Necesito el conjunto de empresas relacionadas de la custodia
  empresas = getattr(c, empresa_relacionada + 'custodia_set')
  resultado = eval('empresas.get(' + empresa_relacionada + ',' + id_empresa + ')')
  if resultado:
      return True
  else:
      return False
```

Acá evidentemente no sabía cómo hacer para "poner" en la llamada a una función el nombre del parámetro que tengo en una variable (empresa\_relacionada en este caso). Siempre leí que el "eval is evil" (además de muy lento en su ejecución), pero hasta ahora no podía evitar esto.

Jugando hace un rato descubrí que puedo hacer esto!

```
def has_empresa_relacionada_custodia(id_custodia, empresa_relacionada='transportista',
                                   id_empresa):
  """ Devuelve true si la empresa + id de la empresa especificados están relacionados
      con el id_custodia """

  c = Custodia.objects.get(id_custodia)
  # Necesito el conjunto de empresas relacionadas de la custodia
  empresas = getattr(c, empresa_relacionada + 'custodia_set')
  # En la siguiente línea está la magia
  resultado = empresas.get(**{empresa_relacionada : id_empresa})
  if resultado:
      return True
  else:
      return False
```

Groso! Al leer que las funciones con keywords son simples llamadas con
diccionarios, me fijé en esta página:

[http://docs.python.org/ref/calls.html](http://docs.python.org/ref/calls.html)

Y ví que se puede llamar a funciones con \*\* o con \* también.

Es un avance importante, ahora puedo parametrizar con valores de variables las llamadas a funciones, y sin usar el eval() (no usen eval, siempre tiene que haber una manera de no usarlo).

Saludos
Marcelo
