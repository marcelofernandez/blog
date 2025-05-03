---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2008-10-07T10:30:00+00:00"
guid: https://blog.marcelofernandez.info/?p=108
title: Cómo corregir el Adelanto de Hora en Ubuntu
url: /2008/10/como-corregir-el-adelanto-de-hora-en-ubuntu/

---
Hola!

Visto y considerando el [bug](https://bugs.launchpad.net/ubuntu/+source/tzdata/+bug/278419) que surgió este fin de semana con la información de husos horarios y [DSTs](http://es.wikipedia.org/wiki/Horario_de_verano) de la vasta mayoría de sistemas [Linuxes](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=501169), [Mac OSXs](http://discussions.apple.com/thread.jspa?threadID=1742160&tstart=90), celulares, mp3, etc., a partir de las 00:00 hs. de este último 5 de Octubre (y que erróneamente produjo el adelanto de 1 hora), acá voy a postear cómo arreglarlo en forma (relativamente) sencilla, a la espera de las actualizaciones de rigor que hacen los SOs.

En una terminal, tipear:

```
export TZ=America/Argentina/Buenos_Aires
cd /tmp
mkdir tzdata2008g
wget ftp://elsie.nci.nih.gov/pub/tzdata2008g.tar.gz
cd tzdata2008g
tar xvzf ../tzdata2008g.tar.gz
date ; date -u
# aca deberia dar la hora mal
sudo zic southamerica
sudo cp /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
date ; date -u
# aca deberia dar la hora bien
```

Lo bueno es que este método es bastante agnóstico; funciona en todos los Ubuntus y Linuxes que andan por ahí; el único detalle es el no uso del comando 'sudo' en otras distros que hacen uso más efectivo del usuario root, con lo cual habría que hacer un 'su' en su lugar.

Este es un post con fecha de vencimiento, pero me decidí por hacerlo igual ya que en Hardy el paquete nuevo [va a entrar dentro de 7 días aprox](https://bugs.launchpad.net/ubuntu/+source/tzdata/+bug/278419/comments/13). (ya que hay toda una política de testing).

Actualización: Una cosa más e importante: la versión "g" del paquete tzdata2008 sólo posterga el cambio de hora al 19 de octubre de 2008, con lo cual sólo "pateamos para adelante" el problema. La solución definitiva debería venir de una nueva versión "h" (?) que no incluya ningún cambio de hora (al menos por ahora y mientras el gobierno lo decida).

Para saber qué cambios están planificados para nuestra instalación, podemos ejecutar:

```
zdump -v /etc/localtime | egrep "2008|2009"
```

Para crear una versión personalizada de los cambios de hora (sería una solución "definitiva"), ver este post:
[http://linux.org.ar/pipermail/sgo-gral/2007-December/001719.html](http://linux.org.ar/pipermail/sgo-gral/2007-December/001719.html)

De todos modos (y al menos en Ubuntu), el paquete tzdata con las actualizaciones propuestas, que corrigen el problema, debería llegar antes del 19 (alrededor del lunes 13).

Actualización 2: Ya están las actualizaciones mencionadas en el repositorio hardy-updates.

Actualización 3: No se "patea para adelante" el problema. El gobierno parece que [va a adelantar la hora el 19 de Octubre](http://www.clarin.com/diario/2008/10/09/um/m-01777946.htm) nomás....

Saludos
Marcelo
PD: Gracias [TecnicosLinux](http://tecnicoslinux.com.ar/node/305)!
