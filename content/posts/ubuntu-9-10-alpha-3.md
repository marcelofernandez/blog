---
author: mfernandez
category:
  - codear
  - linux
  - ubuntu-ar
date: "2009-07-27T23:00:11+00:00"
guid: https://blog.marcelofernandez.info/?p=396
title: Ubuntu 9.10 - Alpha 3
url: /2009/07/ubuntu-9-10-alpha-3/

---
Anoche estuve probando cerca de una hora el [Alpha 3](http://www.ubuntu.com/testing/karmic/alpha3) de lo que va a ser Ubuntu 9.10. No suelo testear las alphas de Ubuntu en etapas tan tempranas del desarrollo (generalmente porque hay pocas cosas novedosas salvo actualizaciones de infraestructura como el kernel, gcc y X.org), pero en esta oportunidad sabía [qué iba a buscar](http://www.ubuntu.com/testing/karmic/alpha3#New%20Intel%20video%20driver%20architecture%20available%20for%20testing) :-)

Si bien hay pocos cambios visuales (a lo que dice [este artículo](http://www.ubunlog.com.ar/blog/lo-que-veras-en-karmic-koala-ubuntu-9-10/) le agregaría que la configuración del audio [ahora sí va a estar un poco más integrada](http://fedoraproject.org/wiki/Features/VolumeControl) con [PulseAudio](http://www.pulseaudio.org/)), quería comentar que incorpora los últimos y nuevos drivers de Intel 2.8, [publicados como estables esta última semana](http://www.phoronix.com/scan.php?page=news_item&px=NzM5MQ), que al fin elimina completamente el soporte a [DRI1](http://en.wikipedia.org/wiki/Direct_Rendering_Infrastructure)/ [EXA](http://www.rojtberg.net/67/exa-uxa-dri-gem-ttm/), y pone a [DRI2](http://wiki.x.org/wiki/DRI2)/ [UXA](http://www.phoronix.com/scan.php?page=article&item=intel_uxa&num=1) como nuevo esquema de drivers (más, [acá](http://www.fayerwayer.com/2009/04/intel-aplica-guadana-en-su-driver-para-linux/)).

Lo bueno es que (a pesar de que lo probé poco tiempo), todos los problemas que [estuvimos y estamos teniendo](https://bugs.launchpad.net/ubuntu/+source/xserver-xorg-video-intel/+bug/359392) con los drivers Intel parecen haberse ido, **¡y la performance es excelente!** Si, ya que que [glxgears no es una herramienta de _benchmark_](http://wiki.cchtml.com/index.php/Glxgears_is_not_a_Benchmark), pero si ahora mi máquina con Ubuntu 9.04 está tirando algo así como ~700 FPS y con problemas feos como este:

En cambio con Karmic Alpha 3 glxgears está escupiendo ¡ **~4800FPS**!, y este bug no está, además de tener KMS ( [Mode-Setting](http://en.wikipedia.org/wiki/Mode-setting) de video por el núcleo directamente) funcionando perfectamente. Ahora puedo cambiar entre X (modo gráfico) y las consolas de texto en forma instantánea, así:

Por lo tanto, y sólo por esto a pesar de que faltan meses para la próxima versión, para mí ya va a valer mucho la pena actualizar en Octubre a Ubuntu 9.10. :-)

**Actualización:** En [Phoronix](http://www.phoronix.com) son optimistas, pero los resultados de sus tests dan ganador a Ubuntu 9.04 en performance. Lean el artículo [acá](http://www.phoronix.com/scan.php?page=article&item=intel_q309_flakes&num=1).

¡Saludos!
