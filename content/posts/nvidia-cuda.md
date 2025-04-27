---
author: mfernandez
category:
  - codear
  - linux
  - programación
  - python
  - ubuntu-ar
date: "2008-10-24T22:41:00+00:00"
guid: https://blog.marcelofernandez.info/?p=114
title: Nvidia CUDA
url: /2008/10/nvidia-cuda/

---
[![](http://3.bp.blogspot.com/_nDZ247g0qSM/SQSmesw-xtI/AAAAAAAABRE/EhtnyF7zG6U/s320/nvidia_logo.jpg)](http://www.nvidia.com/) Estoy leyendo [un artículo](http://www.linuxjournal.com/article/10184) de la última [Linux Journal](http://www.linuxjournal.com/), referido a la tecnología [CUDA](http://www.nvidia.com/object/cuda_home.html), que sacó hace un tiempito nomás [NVidia](http://www.nvidia.com/). Básicamente se trata de una plataforma de software (compilador + libs + soporte en hardware) para que cualquier programador pueda acceder al enorme poder de cálculo que tienen las tarjetas gráficas actuales, que es mucho mayor que las CPUs que se utilizan todos los días; se comenta que la mejora de performance es de ¡uno a dos órdenes de magnitud como regla general!

Ya hace un tiempo vengo leyendo que están utilizando CUDA para romper [WPA/WPA2](http://foro.hackhispano.com/showthread.php?t=31958), [comprimir videos](http://foro.hackhispano.com/showthread.php?t=31938), [procesamiento paralelo](http://www.osnews.com/story/19872), y hasta [acelerar las operaciones de cálculo de un RAID](http://www.pdsi-scidac.org/publications/papers/IPDPS08_abs.html)\[1\]. ¡Me parece bárbaro!, al fin una nueva "cosa" con la cual jugar, usar y aprender.

En primera instancia creo que la vuelta de la tecnología del viejo y querido " [coprocesador matemático](http://en.wikipedia.org/wiki/Coprocessor#Intel_coprocessors)" viene muy bien, más cuando la arquitectura x86/x86\_64 no deja muchas opciones más al momento de escalar; me parece que es un gran mérito de Nvidia poner esta idea primero (masivamente) en el mercado, y como se ve lo viene explotando a pleno (prensa, fama y fortuna, je).

Si no vieron de qué se trata, péguense una vuelta por el [sitio/showroom virtual de CUDA](http://www.nvidia.com/object/cuda_home.html), que está buenísimo. En cuanto a lo técnico, resumo:  

- No es software libre, pero sí es gratuito (free as in beer, not as in speech).
- Ya está disponible para Linux (sólo x86/x86\_64) y Windows. Mac está en camino (ahora en forma de beta descargable).
- Si bien es ideal para usarlo desde C y C++, ya hay bindings para usarlo desde [Matlab](http://developer.nvidia.com/object/matlab_cuda.html) y ¡ [Python](http://mathema.tician.de/software/pycuda)! :-D
- Si bien es recomendable usar una tarjeta gráfica soportada (Nvidia 8000 o mayor), puede utilizarse en forma emulada.
- Compatibiliad binaria: una vez que genero un programa CUDA, funciona en cualquier tarjeta soportada por la plataforma.
- Ojo, no es "transparente" al programador, ni "mágico". Hay que leer la documentación, entender cómo funciona la arquitectura y escribir desde cero.  

Y qué hace la competencia? Los próximos pasos de [AMD](http://www.amd.com/)/ [ATI](http://www.ati.com/) parecen ser el desarrollo de [Fusion](http://en.wikipedia.org/wiki/AMD_Fusion), que es un approach mucho más general, al igual de Intel, que [está por sacar](http://www.somospc.com/2008/10/22/los-nehalem-para-portatiles-llegarian-a-finales-del-2009) [Nehalem](http://en.wikipedia.org/wiki/Nehalem_%28microarchitecture%29).

Ambos parecen tener la idea de empezar a fabricar CPUs con varios cores especializados, al estilo del pionero [Cell](http://en.wikipedia.org/wiki/Cell_processor), pero manteniendo la milenaria y vetusta arquitectura x86. Lo bueno es que esta solución, aunque más compleja, me atrevo a decir que será más efectiva ya que no tendrá el cuello de botella del puerto [PCI-Express](http://en.wikipedia.org/wiki/Pci-express); aunque parece que falta mucho para ver algo concreto, y es atendible que CUDA tenga como objetivo "sólo multiprocesamiento masivo". El tiempo dirá cómo evoluciona la cosa.

Y bueno... hago público mi bajón de no tener una Nvidia serie 8000 y hacer pruebas. :-(

Saludos  
Marcelo

\[1\] Puede ser aplicado, por ejemplo, al módulo [md](http://linux.die.net/man/4/md) de Linux, el que permite que hagamos RAID por software.
