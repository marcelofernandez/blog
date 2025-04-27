---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
date: "2012-06-02T08:34:56+00:00"
guid: https://blog.marcelofernandez.info/?p=1167
title: Instalando DD-WRT en un TP-Link TL-WR941ND
url: /2012/06/instalando-dd-wrt-en-un-tp-link-tl-wr941nd/

---
[![](/wp-content/uploads/2012/06/TL-WR941ND-01-300x180.jpg)](http://www.tp-link.com/ar/products/details/?model=TL-WR941ND) Uno compra un Router/Access Point estos días con la esperanza de que el firmware (software en el equipo) que tiene cargado sea eficiente, no tenga bugs (o al menos tenga actualizaciones periódicas del fabricante que los corrija), e incorpore muchas capacidades interesantes para sacarle el jugo, como el uso de conexiones VPN, priorización de tráfico, poder regular el poder de la antena, gráficos/estadísticas de uso de la conexión, y muchas "cositas" que a nosotros (la gente técnica) nos gusta aprovechar del dispositivo, todo por el mismo precio.

Pero lo más común es que suceda todo lo contrario. Mi router [TP-Link TL-WR941ND](http://www.tp-link.com/ar/products/details/?model=TL-WR941ND), apenas lo compré, le actualicé el firmware oficial, y de ahí en más, a los dos o tres días de estar prendido, mágicamente "se cae", dejando de responder, acudiendo obligatoriamente a apagar y encenderlo nuevamente. Ni hablar de [actualizaciones posteriores](http://www.tp-link.com/ar/support/download/?model=TL-WR941ND&version=V3#tbl_j), ni funcionalidades "copadas". Casi que me sentí estafado; siendo previo poseedor de un mítico [Linksys WRT54G](http://es.wikipedia.org/wiki/WRT54G), lo cambié por el TP-Link que es de norma 802.11N, por ende prometía más velocidad y alcance en mi red.

Pasaron unos meses, hasta que en el [FLISOL Luján de este año](http://flisol.info/FLISOL2011/Argentina/Lujan), [Efraim](http://www.linkedin.com/pub/efraim-wainerman/b/3b5/3b5) me instaló en el Linksys un firmware libre, [basado en Linux](http://www.dd-wrt.com/site/content/about), llamado [DD-WRT](http://www.dd-wrt.com/) que funciona en muchísimos modelos de Access Points/Routers (más de 200 según la página, aunque seguro son más). De antemano sabía de la existencia de estos proyectos/distros de Linux, aunque ignoraba lo bien logrado que estaba y las muchísimas capacidades que le agregaba automáticamente al tenerlo instalado.

Es por todo eso que tenía pendiente instalar DD-WRT en mi TP-Link para liberarlo... hasta hoy: migración exitosa.

Puedo comentar que:

- El proceso de instalación tiene bastantes particularidades, dependiendo mucho del dispositivo y de la versión del hardware que se tiene; sí, dentro de un mismo dispositivo, hay como diferentes "releases" o versiones del hardware (1.0, 2.0, 3.0...), donde el fabricante agrega/saca características, y puede influir tranquilamente en la versión del firmware a instalar.
- Por lo anterior, sugiero encarecidamente [leer toda la documentación](http://www.dd-wrt.com/wiki/index.php/Installation), foros y wiki disponible, además de tomar todas las precauciones del caso, ya que un error se puede pagar tirando el router al tacho de basura.
- Yendo más a mi situación particular, el router TL-WR941ND figura en el [Router Database del proyecto](http://www.dd-wrt.com/site/support/router-database), y dice que funciona con la versión 15778 del firmware dd-wrt (que entre otras cosas, ¡parece ser del 2010!). Además está dentro de la página de [dispositivos con hardware Atheros soportado](http://www.dd-wrt.com/wiki/index.php/Atheros).
- Yo tengo la versión 3.6 del hardware, lo compré hace algunos meses nomás en Galería Jardín, así que es bastante probable que todavía y por un tiempo haya versiones iguales dando vueltas.
- Lo único que hice fue buscar, en foros, como documentación, [y encontré que el "router database" estaba desactualizado](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=40041&start=345), y había una versión mucho más actualizada y probada que no estaba linkeada en la página (está [en este FTP](ftp://dd-wrt.com/others/eko/BrainSlayer-V24-preSP2/2012/03-19-12-r18777/tplink_tl-wr941ndv3/)).
- La parte más fácil fue el upgrade en sí: sólo tuve que ir a "Upload firmware" en el administrador web del TP-Link, subí el archivo "factory-to-ddwrt.bin" que bajé del FTP, apagar/prender y listo. :-)

Ahora tengo muchísimas más opciones y potencia que antes en mi equipo y la casi certeza de que va a funcionar correctamente. En este mismo FTP también se van subiendo periódicamente las versiones nuevas del dd-wrt (del 2012), separada por equipo/versión de hardware: [ftp://dd-wrt.com/others/eko/BrainSlayer-V24-preSP2/2012/](ftp://dd-wrt.com/others/eko/BrainSlayer-V24-preSP2/2012/)

A disfrutar del Software Libre se ha dicho. :-)

¡Saludos!
