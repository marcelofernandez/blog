---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2008-07-15T23:16:00+00:00"
guid: https://blog.marcelofernandez.info/?p=106
title: VPNs en Ubuntu
url: /2008/07/vpns-en-ubuntu/

---
Buenas... esta es una guía de instalación y configuración de un vínculo punto a punto autenticado y encriptado por medio de una red insegura (como puede ser Internet), denominado más comúnmente [VPN](http://es.wikipedia.org/wiki/Red_privada_virtual). Para su implementación se utilizará [OpenVPN](http://openvpn.net/) sobre el Sistema Operativo [Ubuntu 8.04](http://www.ubuntu.com/), aunque seguramente estos mismos pasos servirán para [Debian](http://www.debian.org/) u otros derivados de ella (tal como lo es Ubuntu).

[![](http://3.bp.blogspot.com/_nDZ247g0qSM/SH3YX_Eo8HI/AAAAAAAABFo/nFrxsPPEUPg/s400/openvpn_logo.png)](http://openvpn.net/) Instalación
La instalación, como sucede con la mayoría del software ya disponible en los repositorios de Ubuntu, es sencilla: una vez en la consola, tipear:

```
mfernandez@saturno:~$ sudo apt-get install openvpn
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
openvpn-blacklist
Suggested packages:
resolvconf
The following NEW packages will be installed:
openvpn openvpn-blacklist
0 upgraded, 2 newly installed, 0 to remove and 0 not upgraded.
Need to get 0B/1439kB of archives.
After this operation, 3228kB of additional disk space will be used.
Do you want to continue [Y/n]?
Preconfiguring packages ...
Selecting previously deselected package openvpn-blacklist.
(Reading database ... 25764 files and directories currently installed.)
Unpacking openvpn-blacklist (from .../openvpn-blacklist_0.1-0ubuntu0.8.04.1_all.deb) ...
Selecting previously deselected package openvpn.
Unpacking openvpn (from .../openvpn_2.1~rc7-1ubuntu3.3_i386.deb) ...
Setting up openvpn-blacklist (0.1-0ubuntu0.8.04.1) ...
Setting up openvpn (2.1~rc7-1ubuntu3.3) ...
* Restarting virtual private network daemon.                                      [ OK ]

mfernandez@saturno:~$
```

A partir de este momento, OpenVPN está instalado en esta máquina, y luego habrá que instalarlo de la misma manera en el otro nodo que funcionará como extremo opuesto. De ahora en más, vamos a llamarle nodo "A" al que funcionará como "Servidor" de VPN y nodo "B" al que será el "Cliente".

Configuración Genérica de OpenVPN
El paquete OpenVPN en Ubuntu está bien integrado a la estructura de directorios estándar de este SO:

- En el directorio /etc/openvpn/ se guardan los archivos de configuración de las diferentes conexiones que se pueden crear. El archivo /etc/openvpn/conexion.conf, por ejemplo, va a contener las instrucciones para establecer un vínculo VPN.
- En el archivo /etc/default/openvpn se configuran los enlaces alojados en /etc/openvpn/ que va a administrar el script de arranque/parada de OpenVPN (ver punto siguiente), y si se va a generar un archivo de "status" o no.
- El archivo /etc/init.d/openvpn sirve para poder arrancar, parar y reiniciar las VPNs configuradas en el archivo /etc/default/openvpn.

La única diferencia entre el nodo A y B residirá en el archivo de configuración, que por ejemplo puede ser /etc/openvpn/conexion-a-A.conf en el Cliente y /etc/openvpn/conexion-desde-B.conf en el Servidor. El resto es igual en ambos hosts.

Archivo Default
Tal como se dijo anteriormente, en /etc/default/openvpn se indican los archivos de configuración (que se traducen en conexiones) que administrará el script /etc/init.d/openvpn. El contenido de este archivo es el siguiente:

```
# This is the configuration file for /etc/init.d/openvpn

#
# Start only these VPNs automatically via init script.
# Allowed values are "all", "none" or space separated list of
# names of the VPNs. If empty, "all" is assumed.
#
#AUTOSTART="all"
#AUTOSTART="none"
AUTOSTART="conexion-a-A"
#
# Refresh interval (in seconds) of default status files
# located in /var/run/openvpn.$NAME.status
# Defaults to 10, 0 disables status file generation
#
STATUSREFRESH=10
#STATUSREFRESH=0
```

Los únicos dos parámetros que existen son AUTOSTART y STATUSREFRESH. El primero puede ser igual a "all", "none" o un listado de archivos que deberán existir en /etc/openvpn, pero sin la extensión .conf \[1\].

Archivo de Configuración Cliente - B
El archivo de conexión de un cliente /etc/openvpn/conexion-a-A.conf será muy similar al siguiente:

```
#
# Sample OpenVPN configuration file for
# home using a pre-shared static key.
#
# '#' or ';' may be used to delimit comments.

# Use a dynamic tun device.
# For Linux 2.2 or non-Linux OSes,
# you may want to use an explicit
# unit number such as "tun1".
# OpenVPN also supports virtual
# ethernet "tap" devices.
dev tun

# Our OpenVPN peer is the office gateway.
remote 74.125.47.103

# 10.1.0.2 is our local VPN endpoint (home).
# 10.1.0.1 is our remote VPN endpoint (office).
ifconfig 10.1.0.2 10.1.0.1

# Our up script will establish routes
# once the VPN is alive.
up ./conexion-a-A.up

# Our pre-shared static key
secret test.key

# OpenVPN 2.0 uses UDP port 1194 by default
# (official port assignment by iana.org 11/04).
# OpenVPN 1.x uses UDP port 5000 by default.
# Each OpenVPN tunnel must use
# a different port number.
# lport or rport can be used
# to denote different ports
# for local and remote.
port 1194

# Downgrade UID and GID to
# "nobody" after initialization
# for extra security.
; user nobody
; group nobody

# If you built OpenVPN with
# LZO compression, uncomment
# out the following line.
; comp-lzo

# Send a UDP ping to remote once
# every 15 seconds to keep
# stateful firewall connection
# alive.  Uncomment this
# out if you are using a stateful
# firewall.
ping 10

# Uncomment this section for a more reliable detection when a system
# loses its connection.  For example, dial-ups or laptops that
# travel to other locations.
; ping 15
; ping-restart 45
; ping-timer-rem
; persist-tun
; persist-key

# Verbosity level.
# 0 -- quiet except for fatal errors.
# 1 -- mostly quiet, but display non-fatal network errors.
# 3 -- medium output, good for normal operation.
# 9 -- verbose, good for troubleshooting
verb 3
```

Es importante prestar atención y adaptar convenientemente los siguientes parámetros:

- remote y port. Básicamente indica la IP y el puerto del host (respectivamente) correspondientes a la red insegura contra el cual queremos establecer la VPN. El puerto 1194 es el default de OpenVPN.
- ifconfig. Indica la IP virtual de ambos extremos de la VPN.
- up. Es el script bash que se ejecuta apenas se levanta la interfaz. Notar que el "./" indica que el script debe estar en el mismo directorio del archivo de configuración, esto es, /etc/openvpn/conexion-a-A.up. Usualmente sirve para establecer la nueva ruta (con el comando route) de la interfaz, aunque puede servir para cualquier cosa.
- secret. Es el archivo con la clave pre-compartida (pre-shared) que ambos nodos deben poseer para que la conexión se inicie. Obviamente deben coincidir en ambos extremos, y se recomienda que una vez generado en uno, se transfiera a otro por un canal seguro, como SSH, por ejemplo. Este archivo también debe residir en el mismo directorio del archivo .conf.
- ping. Dado que OpenVPN trabaja sobre un protocolo no orientado a la conexión como lo es UDP, es probable de que si hay firewalls entre ambos extremos de la conexión, la conexión sea filtrada por el mismo al no haber tráfico. El parámetro ping evita esto, haciendo que se envíe tráfico para que la conexión nunca se caiga.

Creación de la Clave Pre-Compartida
Como se dijo anteriormente, la clave pre-compartida es un archivo que deberá residir en el directorio /etc/openvpn de ambos nodos, y ser referenciada por el archivo de configuración de cada uno, por medio de su parámetro secret. La clave se genera con el siguiente comando:

```
root@saturno:/etc/openvpn#  openvpn --genkey --secret test.key
root@saturno:/etc/openvpn# cat test.key
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
7e603bfe70074e19efc3dfd4440c145f
be2bbcc7812ce409780819b78e60bf66
8df944d7d40c119c4d9075a750e2d5e8
8289d67628faa69b0c4cb531727e4c64
145d0c4365ff0216963b3d67084225f2
e764facf91f248385152c8ccfcf93e40
ffb8aa19173bf4c12da08337f30d7f1f
61ea30ef51518276cc8fac51ac0793cd
8ba9417a68ca07ca400a72579aa82036
c15ccb5e470b4571ac58ef03c3d3144f
c8d586a30901fb7401d7d982d8805325
48c39f9a10ecba889ee3eeed3893fb98
2220804f860d100f9d7f602ee50c20ee
986e077958b111fbb57a0cdafa6a4ecd
0b59483d02b5185cf6abb9a83af948fe
61393ca6b50137257b255a296ff844e2
-----END OpenVPN Static key V1-----
root@saturno:/etc/openvpn#
```

Para pasar este archivo al equipo que funcionará como extremo opuesto de la VPN, se sugiere (nuevamente) un medio de transporte confiable, como pen drives, CDs, o en su defecto, SSH. El comando scp test.key marcelo@74.125.47.103:~/ copiará por medio de SSH el archivo test.key al equipo en 74.125.47.103, y para hacerlo, se logueará con el usuario "marcelo", dejando el archivo en su home.

Script de inicio
El script bash de inicio que se muestra a continuación agrega como ruta a cualquier host del rango 10.1.0.0-254

```
root@saturno:/etc/openvpn# cat conexion-a-A.up
#!/bin/sh
route add -net 10.1.0.0 netmask 255.255.255.0 gw $5
```

Archivo de Configuración Servidor - A
Como se dijo anteriormente, lo único que varía en definitiva en el Servidor VPN ("A" en este caso) es el archivo de configuración. Así y todo, las diferencias son sutiles (es decir, muy pocas). El archivo de configuración /etc/openvpn/conexion-desde-B.conf sólo difiere del que está en el cliente en los siguientes parámetros:

```
[...]
# *** El parámetro remote sólo se usa en el cliente!!!! ***
# remote 190.210.30.194

# 10.1.0.1 is our local VPN endpoint (office).
# 10.1.0.2 is our remote VPN endpoint (home).
ifconfig 10.1.0.1 10.1.0.2

# Our up script will establish routes
# once the VPN is alive.
up ./conexion-desde-B.up
[...]
```

En resumen, las diferencias son:

- remote no se usa en el servidor (sólo se usa en el cliente).
- El orden de las IPs virtuales a utilizar con el parámetro ifconfig se invierte, como es lógico.
- El script a ejecutar cambia.

Con respecto al script que configura las rutas, es bueno saber que sin que haga falta ejecutar script alguno, la ruta 10.1.0.2 -dev tun0 se levanta automáticamente en el servidor (y lo mismo sucede con la 10.1.0.1 en el cliente). La configuración de la ruta para la red 10.x.x.x en el cliente permite mapear el resto de IPs 10.x.x.x que pueden existir detrás del Servidor VPN, con el objetivo de que el cliente las vea y pueda comunicarse por estos hosts por medio del Servidor VPN. En resumen, si sólo se pretende comunicar dos extremos (y no hay ninguna red detrás de ninguno de los dos equipos), el route add -net... no hace falta.Ejecución
En resumen, para levantar la VPN, sólo hay que ejecutar tanto en el servidor como en el cliente:

```
mfernandez@B:~$ sudo /etc/init.d/openvpn start
* Starting virtual private network daemon.
* conexion-a-A (OK)                                                            [ OK ]
mfernandez@B:~$

mfernandez@A:~$ sudo /etc/init.d/openvpn start
* Starting virtual private network daemon.
* conexion-desde-B (OK)                                                        [ OK ]
mfernandez@stimpy:~$
```

Conclusión
Si bien no es algo muy sencillo de comprender para alguien ajeno a las redes, para el profesional informático en cambio las VPNs representan una herramienta importantísima para resolver problemas de conectividad como la indisposición de IPs públicas, manteniendo una seguridad y flexibilidad difíciles de igualar. Si combinamos eso con un software multiplataforma, libre y de configuración extremadamente sencilla como OpenVPN, tenemos un ganador. :-)

\[1\] En realidad, si al levantar OpenVPN no existe algún archivo .conf, no importa, el script de inicio muestra un warning avisando que no existe y sigue adelante con el resto de las conexiones definidas en el archivo.

Quejas, dudas, comentarios e insultos? Comenten este post o manden un bonito correo, gracias! :-D

Saludos
Marcelo
