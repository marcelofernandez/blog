---
author: mfernandez
category:
  - codear
  - linux
date: "2006-11-30T13:26:00+00:00"
guid: https://blog.marcelofernandez.info/?p=32
title: Una distro distinta - GoboLinux
url: /2006/11/una-distro-distinta-gobolinux/

---
Por fin algo muy novedoso bajo el sol!

Estuve leyendo sobre una nueva distro, [GoboLinux,](http://www.gobolinux.org/) que innova en el campo de las distros linux, no implementando 'un nuevo sistema de paquetes'. Simplemente se replantearon la vieja estructura de directorios Unix que existe en Linux y se propusieron cambiarlo, para que cualquiera ("hasta mi vieja") lo entendiera, "a la Mac OSX".

Básicamente tienen un esquema de directorios:

```
>~]cd /
ls
Programs
Users
System
Files
Mount
Depot
```

y es compatible hacia atrás con todos los programas, ya que la "vieja" estructura de directorios (/bin, /usr, /var, etc.) están ocultas con un módulo del kernel que ellos mismos desarrollaron, llamado [gobohide](http://www.gobolinux.org/index.php?page=doc/articles/gobohide). Sin embargo, siguen existiendo, y el resto del cuento lo manejan con enlaces simbólicos.

Trabaja con casi todos los sistemas de archivos conocidos, y dispone de un comando para mostrar y ocultar directorios.

Qué lindo sería levantar un Linux y hacer esto:

```
/] cd /Programs
/Programs] ls

AfterStep     E2FSProgs    Htop        NTP          Subversion
ALSA-Driver   Ed           HTTPD       OpenOffice   Sudo
ALSA-Lib      Eject        Hydrogen    OpenSSH      Swfdec
ALSA-OSS      Elinks       IBM-Java2   OpenSSL      Synaptics
ALSA-Utils    Ethereal     ID3Lib      Pango        SysFSUtils
Ardour        Expat        IEEE80211   Patch        Sysklogd
Audacity      File         IMLib2      Perl         TCL
Aumix         Firefox      InetUtils   Pkgconfig    TeTeX
Autoconf      Flac         Intltool    PodXTPro     Texinfo
Automake      Flex         IpodSlave   Popt         TIFF
Bash          Fontconfig   Iptables    PPP          TiMidity++
```

Me encantaría traducir [este artículo](http://www.gobolinux.org/index.php?page=at_a_glance) y postearlo acá, pero lamentablemente no tengo tiempo.

Qué bueno sería que Ubuntu lo incluyera!!!!

Soñar no cuesta nada. Abrí una nueva feature en el [launchpad de ubuntu](https://blueprints.launchpad.net/distros/ubuntu), acá:

[https://blueprints.launchpad.net/distros/ubuntu/+spec/system-directory-approach](https://blueprints.launchpad.net/distros/ubuntu/+spec/system-directory-approach)

Opinen!

Salutes
Marcelo
