---
author: mfernandez
category:
  - codear
  - linux
  - tests
  - ubuntu-ar
date: "2008-05-29T01:42:00+00:00"
guid: https://blog.marcelofernandez.info/?p=102
title: D-Link DWA-510 en Ubuntu 8.04
url: /2008/05/d-link-dwa-510-en-ubuntu-804/

---
Hola!

Si andan buscando una placa WirelessPCI [802.11b/g](http://es.wikipedia.org/wiki/802.11) que funcione perfectamente en Linux (y Ubuntu 8.04 para AMD64 para ser más precisos), la [D-Link DWA-510](http://www.dlinkla.com/home/productos/producto.jsp?idp=1009) funciona perfectamente.

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/SD4V3RZredI/AAAAAAAABD4/Zny3blkRH-c/s400/D-Link_caja.jpg)](http://1.bp.blogspot.com/_nDZ247g0qSM/SD4V3RZredI/AAAAAAAABD4/Zny3blkRH-c/s1600-h/D-Link_caja.jpg) Esta es la caja en la que vino....

Abrí mi PC, enchufé la placa en un slot PCI libre, arranqué mi Ubuntu, y ya estaba la dichosa barra de calidad de señal. No tuve que instalar drivers ni nada!

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/SD4XihZreeI/AAAAAAAABEA/_StJ_bH3RC4/s400/Wireless_sshot.png)](http://2.bp.blogspot.com/_nDZ247g0qSM/SD4XihZreeI/AAAAAAAABEA/_StJ_bH3RC4/s1600-h/Wireless_sshot.png) Pude configurarle una conexión WPA2 sin problemas, así que si bien leí por ahí que en modo AP todavía no está soportado (tengo que corroborarlo), al menos como uso "normal", sirve perfectamente. De más está decir que también soporta WAP, WEP y sin encriptación :-P

Lo bueno es que el chipset que trae este D-Link es un [RaLink](http://www.ralinktech.com/) RT2561, que [está totalmente soportado](http://en.wikipedia.org/wiki/Ralink) [por el fabricante](http://www.ralinktech.com/ralink/Home/Support/Linux.html), a tal punto que liberaron hace tiempo como [GPL el código de los drivers](http://en.wikipedia.org/wiki/Comparison_of_open_source_wireless_drivers) (sin acuerdo [NDA](http://es.wikipedia.org/wiki/NDA) de por medio) y permiten la distribución (sin que se modifique) del firmware que los hace funcionar. Mejor aún es saber que [estos drivers](http://rt2x00.serialmonkey.com/) se encuentran en el núcleo oficial de Linux a partir del 2.6.24, por lo tanto, ya está en Ubuntu Hardy 8.04...

El módulo que lo hace funcionar es el 'rt61pci', y aparece en mi lspci así:

05:07.0 Network controller: RaLink RT2561/RT61 rev B 802.11g

Por otra parte, la placa me salió un poquitito arriba de los $100 en Galería Jardín (U$S 30 aprox.), con lo que si bien no es una "ganga", es una marca reconocida, y funciona 10 puntos en Linux. Además, dudo que hubiera podido conseguir una placa de este tipo mucho más barata.

Además, al ser los drivers GPL, no me tengo que preocupar porque el fabricante cambie de dueño o de parecer, con lo que mi placa siempre va a funcionar (o siempre voy a tener la posibilidad de hacerla funcionar).

También sé que la versión "final" de Ubuntu Hardy 8.04 [tenía algunos](https://bugs.launchpad.net/ubuntu/+source/linux-backports-modules-2.6.24/+bug/194650) [problemas](https://launchpad.net/ubuntu/+bugs?field.searchtext=rt61&orderby=-importance&search=Search&field.status%3Alist=NEW&field.status%3Alist=INCOMPLETE_WITH_RESPONSE&field.status%3Alist=INCOMPLETE_WITHOUT_RESPONSE&field.status%3Alist=CONFIRMED&field.status%3Alist=TRIAGED&field.status%3Alist=INPROGRESS&field.status%3Alist=FIXCOMMITTED&field.assignee=&field.bug_reporter=&field.omit_dupes=on&field.has_patch=&field.has_no_package=) que por suerte ya fueron solucionados mediante actualizaciones del kernel (específicamente la versión 2.6.24-17 que subieron al repositorio principal este último lunes 26).

Bueno, asi que ahora a disfrutar de la red sin cables... :-)

Saludos!  
Marcelo
