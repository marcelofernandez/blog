---
author: Marcelo
category:
  - codear
  - linux
  - ubuntu-ar
date: "2008-04-28T00:57:00+00:00"
guid: https://blog.marcelofernandez.info/?p=100
title: Ubuntu 8.04
url: /2008/04/ubuntu-804/

---
Bueno, hace bastante que no posteo nada, simplemente porque estoy en etapa final de mi Trabajo Final de Licenciatura, y estoy a full!. Por lo tanto, brevemente comento que actualicé a Ubuntu Hardy 8.04, y todo fue muy bien!

Comentarios:  
1 - Por primera vez me anduvo 10 puntos el "actualizador" gráfico, lo que creo que es un golazo a nivel de facilidad de uso y experiencia al usuario.

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/SBUiTwMv37I/AAAAAAAAA_M/klNC49n0xkc/s400/Actualizador.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/SBUiTwMv37I/AAAAAAAAA_M/klNC49n0xkc/s1600-h/Actualizador.png)  


2 - Ahora el chequeo periódico de mantenimiento de los discos al arranque se hace gráficamente (aunque en inglés), y se puede cancelar.

3 - Si no les gusta que las particiones se muestren como "Soporte de xx GiB" (como a mí), lean esto:  
[https://help.ubuntu.com/community/RenameUSBDrive](https://help.ubuntu.com/community/RenameUSBDrive)  
[https://bugs.launchpad.net/ubuntu/+source/gvfs/+bug/190366](https://bugs.launchpad.net/ubuntu/+source/gvfs/+bug/190366)  
Básicamente hay que ponerle una etiqueta a la partición, si alguien no sabe inglés me escribe (o comenta este post) y después posteo los pasos cuando tenga tiempo. :-)

4 - El Firefox 3 Beta 5 anda rápido, pero perdí algunos plugins (Google Reader/Calendar Notifier y Tab Mix Plus), esperemos que rápidamente se actualicen. Otra cosa que noto es que cuando veo páginas en flash está algo propenso a bloquearse unos instantes (toda la aplicación) y seguir; supongo que es porque estoy usando AMD64 y el nspluginwrapper (Adobe, para cuándo un Flash de 64 bits?). Igual flash anda muy bien. El OpenJDK 6 que trae interpreta correctamente un 80% de los applets que probé (cálculo a ojo); igual es mejor que el "IcedTea" de Gutsy, que directamente no servía y si uno quería ejecutar applets necesitaba un navegador de 32 bits (Sun, para cuándo un Webstart y un Plugin de Java para AMD64?).

5 - Me mordió este bug+workaround, relacionado al escanear con mi Multifunción Samsung SCX-4200 (la impresión anda 10 puntos):

[https://bugs.launchpad.net/ubuntu/+source/sane-backends/+bug/220755](https://bugs.launchpad.net/ubuntu/+source/sane-backends/+bug/220755)

6 - Tracker funca de 10 ahora. Lástima que sigo siendo picado por este bug (sí, uso Thunderbird, y qué? :-D ):

[https://bugs.launchpad.net/bugs/148207](https://bugs.launchpad.net/bugs/148207)

7 - Al fin puedo ejecutar Compiz en distintos usuarios/sesiones a la vez!!!! (Nvidia/Compiz, gracias! Mi novia se cansó de pedirme que quería "los efectitos" cuando le dejaba la PC).

8 - Tuve que emparchar los módulos vmnet y vmmon de VMWare 6.0.3 porque no funciona con el kernel 2.6.24 de Hardy:

[http://igordevlog.blogspot.com/2008/03/vmware-603-in-ubuntu-hardy-804-kernel.html](http://igordevlog.blogspot.com/2008/03/vmware-603-in-ubuntu-hardy-804-kernel.html)

Creo no olvidarme nada, pero la verdad es que no tuve ningún gran problema, todo fue 10 puntos y muy sencillo, me animaría a decir que cualquiera con conocimientos de PC lo hubiera hecho. Si luego me encuentro con algún problema relacionado al upgrade, actualizo este post.

Actualización 1: También me afectó [este bug](https://bugs.launchpad.net/ubuntu/+source/linux-restricted-modules-2.6.24/+bug/186382) (sólo para usuarios NVidia), pero la solución [es muy sencilla](http://tombuntu.com/index.php/2008/04/28/workaround-for-pink-shadows-with-compiz/). Seguramente saldrá una actualización corrigiéndolo.

Actualización 2: Pulse Audio es un caño. Como tengo una SB Live PCI (y se escucha mucho mejor que con mi placa onboard), la pobre plaquita onboard estaba desactivada por setup. Ahora con PulseAudio puedo elegir qué stream de audio va por qué placa, con lo que puedo usar los auriculares con la tarjeta onboard y al mismo tiempo los speakers Creative 2.1 conectados a la SB. :-)

Bueno, tengo que seguir escribiendo. Hasta luego.

Marcelo  
