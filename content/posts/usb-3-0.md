---
author: Marcelo
category:
  - codear
  - linux
  - ubuntu-ar
date: "2008-12-15T11:18:00+00:00"
guid: https://blog.marcelofernandez.info/?p=117
title: USB 3.0
url: /2008/12/usb-30/

---
[![](http://2.bp.blogspot.com/_nDZ247g0qSM/SUZchhZ3DEI/AAAAAAAABvU/azv0EDc3BMw/s320/firewire.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/SUZchhZ3DEI/AAAAAAAABvU/azv0EDc3BMw/s1600-h/firewire.png) Más allá de que el [video de la señorita de Intel](http://video.google.com/videoplay?docid=-6015621370324169356&hl=en) lo pueden encontrar en todos los sitios de noticias (me hace acordar a [Chloe de 24](http://www.imdb.com/name/nm0707476/), je), es muy interesante el [hilo de comentarios](http://hardware.slashdot.org/article.pl?sid=08%2F12%2F13%2F2032211) de [Slashdot](http://www.slashdot.org/), similar a lo que hablaba ayer con un amigo sobre [USB 2.0](http://en.wikipedia.org/wiki/USB) vs. [Firewire](http://en.wikipedia.org/wiki/Firewire) (aka IEEE 1394). USB está diseñado para ser muy dependiente del CPU (no por nada lo hizo Intel, guiño, guiño, aunque tiene el _side-effect_ de ser un poco más barato), y Firewire (más dependiente de hardware específico, y por ende un poco más caro) le gana en la práctica por lejos. USB tiene un ancho de banda máximo (teórico) de 480 Mbps, mientras que Firewire viene en _sabores_ de 400 Mbps a 800 Mbps (y ya están saliendo productos de 1600 y 3200 Mbps).


Tratando de ser breve, copio un comentario de Slashdot:  
" _This is why you can't have USB-to-USB devices like you have Firewire-to-Firewire devices. It's why USB is very bad for time-sensitive data like music and video, because you're always waiting for the host controller to do something on the CPU, which might be busy.. have you ever seen DMA to memory ever work properly on consumer grade hardware anyway?_

_It's not so much a "scam" as it is designing to the market. Firewire devices have a non-triv__ial price premium because of the device-to-device controller... but that's why they can do things like daisy-chain or direct connect between computers with no special cables. On the other hand USB allows endpoint devic__es to be made very cheaply.. they have near-zero intelligence if you want. The USB host can be as__"thick" or "thin" as the OEM wants... they can put all the host chip control in software drivers to keep chip cost down. They can also put all the control codes for devices in software... remember "wINKjets" that went obsolete with each new Windows version... they have almost no internal software at all._"

Por motivos como estos, por ejemplo, hace poquito compré una [sintonizadora barata de TV](http://www.encore-usa.com/product_item.php?region=us&bid=2&pgid=17&pid=45), de tipo [PCI](http://en.wikipedia.org/wiki/PCI_Local_Bus), y Ubuntu la tomó sin ningún problema; funciona perfecto, sin ningún "salto", y sin consumir CPU (el cuello de botella para [_encodear_](http://en.wikipedia.org/wiki/Video_coding) al vuelo pasó a ser mi CPU, je).

Si la hubiera comprado USB (sólo a $60 más).... hubiera tenido otro problema más además del encoding: el consumo de CPU de la misma transferencia de datos quizás me hubiera impedido hacer otras cosas mientras transfería video. Obviamente, al comprarlo PCI tengo que pagar el costo de no poder llevar la sintonizadora de un equipo a otro fácilmente, cuestión que no necesitaba tampoco.

Es por esto que algunas filmadoras [DV](http://en.wikipedia.org/wiki/DV) que conozco vienen exclusivamente con interfaz Firewire, teniendo como alternativa una interfaz USB de compromiso (entiéndase "compromiso" como de "menor calidad", como está documentado). Y el tema del precio mayor no es tan así hoy... en Capital se pueden conseguir controladoras Firewire PCI a $46.

[![](http://3.bp.blogspot.com/_nDZ247g0qSM/SUZcB7Mnm9I/AAAAAAAABvM/yagpi07iSiY/s320/usb.png)](http://3.bp.blogspot.com/_nDZ247g0qSM/SUZcB7Mnm9I/AAAAAAAABvM/yagpi07iSiY/s1600-h/usb.png) Así y todo, USB es el rey, porque Intel domina claramente el mercado, logró que en su momento todos los fabricantes de motherboards y dispositivos incluyeran dispositivos USB, y porque USB es suficientemente bueno (aka " [good enough](http://en.wikipedia.org/wiki/Principle_of_good_enough)") en la mayoría de los casos (no es la primera vez que pasan estas cosas en la informática). Algo más a destacar es que al parecer USB 3.0 elimina el [polling](http://en.wikipedia.org/wiki/Polling_%28computer_science%29) de dispositivos por parte del CPU, con lo cual el consumo de procesador bajaría considerablemente (pero hay que verlo en acción, Firewire en cambio es una realidad ya probada, donde sus versiones más veloces apenas requieren cambios estructurales).

En fin, como siempre a Slashdot lo elevan muchísimo los comentarios de muy alto nivel. Veremos cómo funciona USB 3.0 (especificación disponible [aquí](http://www.usb.org/developers/docs/)).

Saludos  
Marcelo  
PD: Qué lindo que las demostraciones las hagan con Linux (Ubuntu); para que se den una idea de la importancia de Linux hoy, USB 1.0 tardó mucho tiempo en soportarse en Linux (1999). Hoy el mismo desarrollo de USB 3.0 se hace sobre Linux...
