---
author: Marcelo
category:
  - codear
  - python
  - ubuntu-ar
  - web
date: "2009-09-14T05:41:36+00:00"
guid: https://blog.marcelofernandez.info/?p=433
title: PyCon Argentina 2009, Cappuccino y LLVM
url: /2009/09/pycon-argentina-2009-cappuccino-y-llvm/

---
Me gustaría hacer un post bien largo acerca de todo lo que me dejó personalmente la última [PyCon Argentina](http://ar.pycon.org/2009/about/), pero lamentablemente estoy complicado con el tiempo, ya que no sólo quiero leer y escribir acerca de todo el "bombardeo" de información que te deja cada charla, sino que también quisiera investigar un poco cada cosa y dejar acá algo más que solamente los links. Pero bueno, vamos a hacer el intento de resaltar lo primero que me viene a la mente.

### Cappuccino y la Web que se viene [![cappuccino-icon](/wp-content/uploads/2009/09/cappuccino-icon-150x150.png)](http://cappuccino.org)

Uno de los muy interesantes punteros que me dejó la conferencia tiene que ver con las keynotes\[1\]\[2\], y la existencia por un lado de la aplicación web [280Slides](http://280slides.com/) y por el otro del framework en el cual se basa, [Cappuccino](http://cappuccino.org/). Sugiero pegarle una mirada, más que nada a las [demos](http://cappuccino.org/learn/demos/) (para los faltos de tiempo como yo); me parece que es **sencillamente genial,** hay mucha influencia del [OS X](http://www.apple.com/es/macosx/) acá, y se nota. **Es una aplicación donde la línea entre lo desktop y web se esfuma, literal y definitivamente**. Lástima que hay que aprender oootro lenguaje nuevo, [Objective-J](http://cappuccino.org/learn/).Y esto parece que no se va a detener, tal como dijo [Jacob](http://jacobian.org/). Más si nos enteramos que hay [desarrollos bastante avanzados](http://games.slashdot.org/story/09/09/13/1820230/Initial-WebGL-Support-Lands-In-WebKit) para integrar contextos canvas 3D en los navegadores, y leemos algunas características futuras de [HTML5](http://en.wikipedia.org/wiki/HTML_5#New_APIs). En resumen, **él plantea que toda la** _**toolchain web**_ **actualmente** _**apesta**_ **, incluso Python**, y propone algunas líneas de solución para que Python sea _el_ lenguaje web del 2020, cuando HTML 5 sea el estándar y el navegador web sea [el nuevo Sistema Operativo](http://www.infoworld.com/d/developer-world/are-operating-systems-doomed-086). [Ya había escrito](/2009/03/novedades-en-firefox-31-beta-sera-35/) algo de esto.

### Código más rápido, código más optimizado: LLVM [![DragonSmall](/wp-content/uploads/2009/09/DragonSmall.png)](http://www.llvm.org)

De la segunda plenaria, me quedó claro que al fin surgió un contendiente de relativa importancia al venerable [GCC](http://gcc.gnu.org/), y se llama [LLVM](http://llvm.org/). Acá tengo más todavía para investigar, pero básicamente el _branch_ _performante_ de Python llamado [Unladen Swallow](http://code.google.com/p/unladen-swallow/) implementará (entre varias cosas más) un [JIT](http://es.wikipedia.org/wiki/Compilación_en_tiempo_de_ejecución) para [CPython](http://en.wikipedia.org/wiki/Python_(programming_language)#CPython) implementado por medio de LLVM; algo así como [V8](http://code.google.com/p/v8/) o [TraceMonkey](https://wiki.mozilla.org/JavaScript:TraceMonkey) son para Javascript. Una de las cosas en las que Collin afirmó varias veces fue que "no pretendemos ser originales, queremos robar ideas de cualquier paper, de donde sea, con tal de hacer Python más rápido" :-). Lo malo es que al parecer, y si bien [era una de las ideas iniciales del proyecto](http://code.google.com/p/unladen-swallow/wiki/ProjectPlan#2009_Q3_and_Beyond), eliminar el [GIL](http://docs.python.org/c-api/init.html#thread-state-and-the-global-interpreter-lock) no será tan fácil :-( (de paso y relacionado, ¡excelente charla la de [Multiprocesamiento en Python](http://ar.pycon.org/2009/conference/schedule/event/16/)!).

LLVM está haciendo muchos [méritos para ser considerado](http://llvm.org/ProjectsWithLLVM/), como por ejemplo que [Apple está apostando muy fuertemente por él](http://arstechnica.com/apple/reviews/2009/08/mac-os-x-10-6.ars/9), incluso tiene la idea de reemplazarlo definitivamente dejando atrás a GCC. En Snow Leopard (OS X 10.6), ya mismo se ofrece una interfaz compatible con GCC para LLVM además de toda la _toolchain_ para compilar software desde los fuentes con él. FreeBSD [también está trabajando para migrar a él](http://barrapunto.com/articles/09/05/12/216239.shtml). Y si bien todavía le falta bastante camino por recorrer (tiene [escaso soporte para C++](http://clang.llvm.org/cxx_status.html), o que no compila el kernel de Linux, por ejemplo), ya se pueden ver algunas [muestras de su potencial](http://www.phoronix.com/scan.php?page=article&item=apple_llvm_gcc&num=1), y personalmente encontré varios [comentarios](http://cliffhacks.blogspot.com/2007/03/experimenting-with-llvm.html) de que es mucho más sencillo contribuir y entender el código del proyecto comparado con GCC.

En fin, repetí en ambos párrafos, en asuntos totalmente diferentes, la palabra _toolchain_, que significa literalmente "cadena de herramientas", y está relacionado generalmente al uso de varias herramientas como un conjunto en las diferentes etapas de la creación de aplicaciones. Evidentemente, tanto en el mundo de las aplicaciones web, como en el de los lenguajes de programación, se está yendo hacia la optimización de los recursos que tenemos. ¡Buenísimo! :-D

\[1\] [Snakes on the Web - Jacob Kaplan-Moss](http://jacobian.org/writing/snakes-on-the-web/)
\[2\] [Unladen Swallow - Collin Winter](http://ar.pycon.org/2009/conference/schedule/event/35/)

¡Gracias a [PyAr](http://www.python.com.ar) por una excelente PyCon!
Saludos
