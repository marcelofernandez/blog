---
author: Marcelo
category:
  - codear
  - linux
  - python
  - sysadmin
  - ubuntu-ar
date: "2009-03-10T12:57:00+00:00"
guid: https://blog.marcelofernandez.info/?p=126
title: Otra vez el Cambio de Horario en Ubuntu
url: /2009/03/otra-vez-el-cambio-de-horario-en-ubuntu/

---
[![](http://3.bp.blogspot.com/_nDZ247g0qSM/SbZkYeipkjI/AAAAAAAACL4/U00MPcbKV2c/s320/224752387_1d26835f93_b.jpg)](http://www.flickr.com/photos/trvr3307/224752387/) Este post es sólo para comentar, amigos sysadmins, que [este domingo a las 00:00 hs. se "atrasará" una hora el reloj del lado este del país](http://www.clarin.com/diario/2009/03/09/um/m-01873771.htm), y que todos aquellos que tengan actualizados el paquete [tzdata](http://packages.ubuntu.com/search?keywords=tzdata&searchon=names) van a tener actualizados el reloj automáticamente, a diferencia de otras oportunidades donde lo repentino de los caprichos las decisiones políticas hicieron que hubiera que hacer esto a mano.

Para comprobar esto, pueden correr en la consola:

```
marcelo@marcelo-laptop:~$ zdump -v /etc/localtime | grep 2009
/etc/localtime  Sun Mar 15 01:59:59 2009 UTC = Sat Mar 14 23:59:59 2009 ARST isdst=1 gmtoff=-7200
/etc/localtime  Sun Mar 15 02:00:00 2009 UTC = Sat Mar 14 23:00:00 2009 ART isdst=0 gmtoff=-10800
/etc/localtime  Sun Oct 18 02:59:59 2009 UTC = Sat Oct 17 23:59:59 2009 ART isdst=0 gmtoff=-10800
/etc/localtime  Sun Oct 18 03:00:00 2009 UTC = Sun Oct 18 01:00:00 2009 ARST isdst=1 gmtoff=-7200
```

Las dos primeras líneas representan las reglas con el cambio mencionado. Si no aparece nada después de correr este comando, actualizen el paquete tzdata. :-)

Saludos,
Marcelo
