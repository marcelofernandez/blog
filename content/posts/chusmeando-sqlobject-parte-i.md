---
author: Marcelo
category:
  - codear
  - programación
  - python
date: "2007-07-10T13:32:00+00:00"
guid: https://blog.marcelofernandez.info/?p=73
title: Chusmeando SQLObject - Parte I
url: /2007/07/chusmeando-sqlobject-parte-i/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/RpTixI2NBII/AAAAAAAAAHw/IxLKiWwCU14/s400/camadas.jpg)](http://4.bp.blogspot.com/_nDZ247g0qSM/RpTixI2NBII/AAAAAAAAAHw/IxLKiWwCU14/s1600-h/camadas.jpg) Aprovechando que el [ORM](http://es.wikipedia.org/wiki/Mapeo_objeto-relacional) en Python es noticia (ya que [Canonical liberó](http://www.osnews.com/story.php/18242/Canonical-Begins-Opening-up-Launchpad) [Storm](https://storm.canonical.com/))... comento un par de desventajas del framework para acceder a BDs desde [Django](http://www.djangoproject.com/) y cómo las soluciona [SQLObject](http://www.sqlobject.org/) (específicamente no lo usé, pero estuve leyendo algo...)

Al momento de laburar con Django y la forma en que maneja la BD (definición de los modelos) nos encontramos dos limitaciones graves:

1) No soporte de herencia entre tablas. Es decir, si yo desde mi "mundo real" tengo que representar:

```
class Transporte():
   modelo = ''
   largo = ''
   ancho = ''
   patente = ''

class Auto(Vehiculo):
   tiene_baul = ''

class Camion(Vehiculo):
   tiene_semi = ''
```

No puedo representar esto en Django, ya que no soporta herencia. Por lo tanto, se puede "simular" la herencia usando relaciones "uno a uno" o copiando y pegando. Con la primera
opción caímos en varios problemas, así que tuvimos que "copiar y pegar" las propiedades que deberían haber sido heredadas.

En cambio SQLObject sí soporta herencia; y de dos maneras: usando la [herencia de python](http://docs.python.org/tut/node11.html#SECTION0011500000000000000000) o "componiendo" la herencia.

Este es un ejemplo de composición en un modelo de SQLObject:

```
class Transporte(SQLObject):
    marca = StringCol()
    modelo = StringCol()
    anio = DateCol()
    color = StringCol()
    patente = StringCol()

class Auto(SQLObject):
    transporte = ForeignKey('Transporte')
    tiene_baul = BoolCol()

class Camion(SQLObject):
    tiene_semi = BoolCol()
```

El problema con esta forma de hacerlo es que la clase "Auto" no parece terminar de heredar los atributos de "Transporte", ya que a sus propiedades heredadas hay que accederlas por medio de una propiedad. Pero esto se puede mejorar usando herencia. SQLObject tiene algunas clases que
permiten definir herencia entre clases del modelo.

b) Este es un ejemplo de herencia en SQLObject:

```
from sqlobject.inheritance import InheritableSQLObject

class Transporte(InheritableSQLObject):
    marca = StringCol()
    modelo = StringCol()
    anio = DateCol()
    color = StringCol()
    patente = StringCol()

class Car(Vehicle):
    tiene_baul = BoolCol()
```

Acá está el esquema de BD que genera SQLObject ("abajo del capó"):

```
CREATE TABLE auto (
    id INTEGER PRIMARY KEY,
    child_name TEXT,
    tiene_baul BOOL
);

CREATE TABLE transporte (
    id INTEGER PRIMARY KEY,
    child_name TEXT,
    modelo TEXT,
    marca TEXT,
    anio TEXT,
    color TEXT,
    patente TEXT
);
```

SQLObject agrega de forma "transparente" el atributo "child\_name" a ambas tablas. Esta columna contiene el nombre de la tabla "hija". SQLObject utiliza esta columna para localizar y agregar el contenido de las filas correspondientes en las tablas 'auto' y 'transporte' para generar un objeto "Auto" en python de forma coherente.

SQLObject soporta varios niveles anidados de herencia simple. Herencia múltiple no está soportada (no sé si es mejor que no lo soporte).

Mañana posteo la otra "gran" dificultad que le encontramos al manejarnos con BDs en Django.

Marcelo

Créditos 1: Para algunas cosas de este post me basé en el [libro de TurboGears](http://docs.turbogears.org/1.0/TGBooks). Está muy bueno.
Créditos 2: Imagen extraída del blog de la [comunidad (brasilera o portuguesa?) de TurboGears](http://oturbogears.org/)
