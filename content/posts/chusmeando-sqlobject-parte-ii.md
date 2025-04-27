---
author: mfernandez
category:
  - codear
  - programación
  - python
date: "2007-07-11T13:50:00+00:00"
guid: https://blog.marcelofernandez.info/?p=74
title: Chusmeando SQLObject - Parte II
url: /2007/07/chusmeando-sqlobject-parte-ii/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/RpY3Hh9dfgI/AAAAAAAAAIA/mdQkfrguxEM/s400/g_gear.png)](http://www.turbogears.org) Continuando el post de ayer, otra desventaja del sistema de persistencia de Django es que no soporta atributos en relaciones "Muchos a Muchos". Un caso podría ser Usuario <--> Rol, donde un Usuario puede tener varios Roles y a su vez un Rol ser referenciado por varios usuarios; además, necesito almacenar si alguna combinación usuario<->rol está activa o no. El atributo booleano "activo" sería una propiedad por cada Usuario<->Rol en particular, y esto es lo que no soporta Django con su ManyToManyField.

En cambio SQLObject sí los soporta. Ejemplo:

```
class Usuario(SQLObject):
   class sqlmeta:
       tabla = 'tabla_usuario'
       nombre_usuario = StringCol(alternateID=True, length=20)
       roles = SQLRelatedJoin('Rol',
               intermediateTable='roles_usuario',
               createRelatedTable=False)

class Rol(SQLObject):
   nombre = StringCol(alternateID=True, length=20)
   usuarios = SQLRelatedJoin('Usuario',
              intermediateTable='roles_usuario',
              createRelatedTable=False)

class RolesUsuario(SQLObject):
   class sqlmeta:
      table = 'roles_usuario'
      usuario = ForeignKey('Usuario', notNull=True, cascade=True)
      rol = ForeignKey('Rol', notNull=True, cascade=True)
      activo = BoolCol(notNull=True, default=False)
      unique = index.DatabaseIndex(usuario, rol, unique=True)
```

Como se puede apreciar, al pasarle la propiedad "createRelatedTable=False" al SQLRelatedJoin, el programador puede definir por sí mismo la clase que "une" el Muchos a Muchos y agregarle atributos, como primera medida. Lo segundo es, además de crear los ForeignKey correspondientes, crear el índice de tipo 'unique' sobre los dos atributos FK, usuario y rol. [Más info sobre esto acá](http://www.sqlobject.org/FAQ.html#how-can-i-define-my-own-intermediate-table-in-my-many-to-many-relationship).

No recuerdo alguna(s) otra limitación(es) tan importante(s) como éstas dos de la librería de acceso a BD de Django, pero SQLObject parece "pulenta" para desarrollos serios.

Conclusión:
[TurboGears](http://www.turbogears.org/) es "la competencia" de Django; en mi opinión (por lo poco que ví) en la parte de acceso a BD al incluir SQLObject, parece mucho más maduro que Django. Django no es malo, simplemente le falta madurar (más que nada en la definición de modelos). SQLObject se puede usar aparte, de hecho [tiene un sitio propio](http://www.sqlobject.org/) y se puede utilizar de forma independiente.

Por otra parte, la librería de templates de Django es excelente, acá nos gustó mucho y de hecho lo utilizamos separado de Django en algún que otro sistema con excelentes resultados (sí, se puede utilizar aislado). No nos gustó tanto a la vista el de TurboGears, pero será cuestión de probar de forma más "seria" TG y hacer una evaluación más completa.

Saludos
Marcelo
