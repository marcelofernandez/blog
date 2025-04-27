---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2009-06-06T17:20:54+00:00"
guid: https://blog.marcelofernandez.info/?p=213
title: 'Dos herramientas para Sysadmins: Ninja y KSplice'
url: /2009/06/dos-herramientas-para-sysadmins-ninja-y-ksplice/

---
Navegando por ahí, me acabo de encontrar con dos herramientas interesantes, que me interesa compartir...

## **Ninja**

 [![](/wp-content/uploads/2009/06/ninja_the_last_thing_you_see-150x150.jpg)](http://www.forkbomb.org/ninja/)
La primera llamada [Ninja](http://www.forkbomb.org/ninja/), que viene a ser algo así (traducido de su página web):

> Ninja es un sistema de detección y prevención de [escaladas de privilegios](http://en.wikipedia.org/wiki/Privilege_escalation) para GNU/Linux. Mientras está corriendo, monitorea las actividades de los procesos locales por un lado, y de los procesos corriendo como root por el otro. Si un proceso lanza a otro nuevo con UID o GID cero (root), Ninja va a loguear la información necesaria acerca del mismo, y opcionalmente lo puede finalizar si fue creado por un usuario _(es decir, un proceso de un usuario_) sin autorización.

En mi opinión, si bien hay herramientas [más](http://fedoraproject.org/wiki/SELinux) [complejas](http://en.opensuse.org/AppArmor) para hacer este tipo de cosas con servicios en producción, parece un buen complemento cuando se necesita algo más sencillo y no tan crítico, como dar acceso de terminal o X  a usuarios finales. [Acá](http://blog.bodhizazen.net/linux/how-to-ninja/) hay un post sobre cómo instalarlo en Ubuntu.

## KSplice

 [![](/wp-content/uploads/2009/06/ksplice-150x51.png)](http://www.ksplice.com)
La otra herramienta, promete algo que sería la salvación para todos los sysadmins: **aplicar actualizaciones al núcleo de un SO sin reiniciar el equipo, "al vuelo", en forma transparente.** Esta herramienta de salvación para todos nosotros se llama [KSplice](http://http://www.ksplice.com), es abierta, y fue tema de [investigación](http://www.ksplice.com/paper) y desarrollo por [gente del MIT](http://www.ksplice.com/about). Si realmente anda bien y funciona, sólo falta que salga en [Slashdot](http://www.slashdot.org) (no creo que tarde mucho...).

KSplice funciona creando actualizaciones (aplicables instantáneamente) para el kernel en ejecución. Estas actualizaciones son igual de efectivas que las estándar (supongamos las que provee la distribución que estamos utilizando), con la diferencia de que no hace falta reiniciar para que entren en funcionamiento.

Para esto, necesita el código fuente de la actualización (un parche para el kernel que se está corriendo), más el código fuente del kernel en ejecución. El núcleo **no necesita ser modificado por adelantado** para que esto funcione (sólo x86 y x86\_64).  El proceso de creación y aplicación de un parche es [bastante sencillo](http://www.ksplice.com/example-update), y **¡hasta se puede deshacer!**

Por el momento sólo funciona con Linux x86, x86\_64 y ARM, que su código [se puede bajar](http://www.ksplice.com/software) en forma de fuente, agregar los repositorios GIT, y también descargar una versión binaria [para instalar](http://www.ksplice.com/install) en cualquier distro GNU/Linux.

Las únicas desventajas (si cabe) en cuanto a lo técnico que le veo es que:

- Sólo aplica parches que no cambien la semántica del núcleo (supongo que esto quiere decir que no hacen que cambie su [ABI](http://en.wikipedia.org/wiki/Application_binary_interface)). Sin embargo, ellos mismos afirman que el 88% de los parches de seguridad para núcleo publicados entre Mayo de 2005 y Mayo de 2008 [eran aplicables perfectamente](http://www.ksplice.com/cve-evaluation) con KSplice. Esto hace que sirva en primera instancia para parches de seguridad, y para corrección de fallos en segundo lugar.
- No hay información sobre qué pasa si reinicio el equipo efectivamente con parches aplicados vía KSplice (supongo que todos los cambios se deshacen, ya que esto es al vuelo y no hay modificación sobre la imagen binaria del núcleo en sí mismo).
- Es necesaria la misma versión de compilador (o casi, ya que no hace un chequeo por número estricto de versión, sino que hace un chequeo de posibles conflictos y aborta en caso de detectar uno, afirmando en su web que esto es seguro).

Por otra parte, próximamente van a ofrecer el servicio de actualizaciones, esperemos que cumplan lo que prometen, a mí me cuesta creer que esto sea así de fácil. Sería bueno que todas las distros lo incluyeran e integraran...

Bueno, es todo por ahora.

¡Saludos!

Marcelo
