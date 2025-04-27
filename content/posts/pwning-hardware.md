---
author: mfernandez
category:
  - codear
  - linux
  - personal
  - programación
  - python
date: "2014-11-01T16:00:51+00:00"
guid: https://blog.marcelofernandez.info/?p=1404
title: Pwning hardware
url: /2014/11/pwning-hardware/

---
El video dura media hora, pero a mí me gustó muchísimo, aprendí mucho. [Mikah Scott](http://scanlime.org/ "Scanlime - One girl's diary of improvisational engineering") es [una](http://scanlime.org/resume/ "Mikah Scott - Resume") [genia](http://www.misc.name/ "Micah Scott Art Portfolio"), y se propone investigar cómo customizar el firmware de una lectora/grabadora de CD/DVD/Blu-Ray, para dominarlo por completo, empezando por el microcontrolador ARM que posee. Por ejemplo, moviendo el láser en la posición que uno quiera y dispararlo. Habla un excelente y puntilloso inglés, así que se le entiende palabra por palabra, sugiero que lo vean incluso para practicar su inglés técnico.

Es muy interesante cómo usa Photoshop para visualizar el contenido de un firmware (?!??!?! ¡nunca se me hubiera ocurrido!), y cómo usa [IDA](https://www.hex-rays.com/products/ida/index.shtml "IDA Disassembler") (este sí es más lógico) para interpretar el código binario.

Además, usa [vusb-analyzer](http://vusb-analyzer.sourceforge.net/ "Virtual USB Analyzer") en Ubuntu para visualizar el tráfico USB dumpeado con usbmon o similares, por ejemplo _snifeado_ de una máquina virtual.

Por último, usa iPython para hacer que el ARM y el resto de los chips con los que interactúa ( [mt1939](http://www.mediatek.com/en/products/home-entertainment/optical-disc-drive/blu-ray/mt1939/ "MT1939 - Rewritable Blu-ray drive platform"), dsp) haga lo que uno quiera (todavía está en avance).

Es increíble cómo en el ámbito de seguridad se usa Python (lo confirmé en la [Ekoparty](http://www.ekoparty.org/ "Ekoparty Security Conference") en estos días).

Insisto, se aprende muchísimo viendo este tipo de videos: herramientas, técnicas, trucos y fundamentalmente cómo abordar estos desafíos.

Saludos
