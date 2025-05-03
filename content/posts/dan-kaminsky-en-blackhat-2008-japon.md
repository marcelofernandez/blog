---
author: Marcelo
category:
  - codear
  - linux
  - python
  - sysadmin
  - ubuntu-ar
  - web
date: "2008-12-27T15:05:00+00:00"
guid: https://blog.marcelofernandez.info/?p=120
title: Dan Kaminsky en BlackHat 2008 - Japón
url: /2008/12/dan-kaminsky-en-blackhat-2008-japon/

---
[Esta presentación](http://www.blackhat.com/presentations/bh-jp-08/bh-jp-08-Kaminsky/BlackHat-Japan-08-Kaminsky-DNS08-BlackOps.pdf) es la única que me llamó la atención repasando [el archivo](http://www.blackhat.com/html/bh-japan-08/brief-bh-jp-08-archives.html) del [Black Hat de Octubre de este año en Japón](http://www.blackhat.com/html/bh-japan-08/bh-jp-08-main.html). El ya conocido [Dan Kaminsky](http://www.doxpara.com/?page_id=1159) fue el keynote del evento, explicando no sólo (nuevamente) la vulnerabilidad de DNS descubierta por él este año, sino también qué se viene en general en materia de (in)seguridad informática.

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/SVZEa1_uKvI/AAAAAAAAB0o/oouIMK0iHIg/s400/hd_logo01.gif)](https://www.blackhat.com/html/bh-japan-08/bh-jp-08-main.html)  
Son un montón de diapositivas (101!), unas cuantas las pasé por alto, pero son útiles; a modo de resumen, enumero los " _highlights_":  

- **Vulnerabilidad DNS**: Análisis exhaustivo del [problema](http://securosis.com/2008/07/08/dan-kaminsky-discovers-fundamental-issue-in-dns-massive-multivendor-patch-released/), [demostración del ataque](http://www.caughq.org/exploits/CAU-EX-2008-0002.txt). Es útil porque empieza desde el principio, comenta la impresionante ( _awesome_) reacción de las " _software factories_" de DNS con un lindo grafiquito, y tiene interesantes conclusiones para los desarrolladores de software en general (ver los _Takeaway_ a lo largo de toda la presentación). También explica **porqué atacar DNS: ¡es el [MiTM](http://en.wikipedia.org/wiki/Man-in-the-middle_attack)** perfecto!.
- **Lo que viene**: Bienvenidos a la tercera era del hacking. Ya se explotaron vulnerabilidades en los servidores y en los navegadores. Lo que sigue es aprovecharse del resto del software, que está íntimamente ligado a la red también; a modo de ejemplo cita [esta vulnerabilidad](http://www.microsoft.com/technet/security/bulletin/ms07-048.mspx) del Sidebar de Windows Vista. Incluye a los juegos en red, y a las formas "seguras" de actualizar desde el software que se usa a diario.  

- **Cuidado con SSL**: Destaca muchos problemas a futuro, tanto técnicos como del usuario: falta de escalabilidad y de difusión, problemas con los virtual hosts, los usuarios no prestan atención a los mensajes de advertencia, el uso de MD5 para su firma, etc. "No es la panacea que pareciera ser".  

- **Autenticación - el Bug principal de 2008**: elabora una lista las últimas vulnerabilidades importantes conocidas, todas tienen que ver con este punto crucial en la interacción de sistemas.
- En resumen, la infraestructura y servicios (¡todos ellos!) de Internet y las Intranets dependen del servicio de DNS. Hay que mejorar la infraestructura; esta vez tuvimos suerte, la próxima vez puede que no sea tan "suave" el problema. DNS no debería haber sido capaz de provocar este daño: entonces, ¿Por qué lo fue?

Además, mientras tuve algo de tiempo, también estuve mirando:

[The Internet is Broken: Beyond Document.Cookie - Extreme Client Side Exploitation](https://www.blackhat.com/presentations/bh-jp-08/bh-jp-08-McFeters/BH_US_08_Mcfeters_Carter_Heasman_Extreme_Client-Side_Exploitation.pdf). Presenta una serie de vulnerabilidades en varios componentes de software comunes estos días, provocado por el abuso de confianza en el nombre de dominio (está claramente relacionado con lo que decía Kaminsky, pero bien práctico); en Java, los servidores Web locales de ciertas aplicaciones, etc.

[Disclosing Secret Algorithms from Hardware](https://www.blackhat.com/presentations/bh-jp-08/bh-jp-08-Nohl/BlackHat-Japan-08-Nohl-Secret-Algorithms-in-Hardware.pdf): Confieso que no sé mucho de electrónica, pero así y todo entiendo que esta charla estuvo interesantísima: ¡los tipos hacen ingeniería reversa de algoritmos en hardware! Muestran cómo se hace, en forma muy gráfica, y explican qué herramientas utilizan. Me asombra que digan "acá hay un NAND, así y así..." Y ya que están, agarran un chip [RFID](http://en.wikipedia.org/wiki/RFID) de [MiFare](http://www.mifare.net/) a modo de (mal) ejemplo para determinar el algoritmo de encriptación (cerrado), como [ya lo hicieron anteriormente](http://www.defcon.org/images/defcon-16/dc16-presentations/defcon-16-anderson-ryan-cheisa.pdf) (aunque [la ley impidió la presentación en sí](http://yro.slashdot.org/article.pl?sid=08/08/09/1812256)). Nobleza obliga, debo agregar que la compañía relativiza esta cuestión y [hace pública](http://www.mifare.net/security/mifare_classic.asp) su posición al respecto.

Bueno, espero que tengan un feliz año!

Saludos  
Marcelo
