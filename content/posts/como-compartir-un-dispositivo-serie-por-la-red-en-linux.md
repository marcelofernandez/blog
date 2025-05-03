---
author: Marcelo
category:
  - codear
  - linux
  - programación
  - sysadmin
date: "2013-05-27T17:12:54+00:00"
guid: https://blog.marcelofernandez.info/?p=1314
title: Cómo compartir un dispositivo serie por la red en Linux
url: /2013/05/como-compartir-un-dispositivo-serie-por-la-red-en-linux/

---
En el trabajo me encontré con la necesidad de utilizar un puerto serial (por ejemplo, un dispositivo con un adaptador USB/Serie en /dev/ttyUSB0, un módem 3G o una placa [Arduino](http://www.arduino.cc/ "Arduino - HomePage") en /dev/ttyACM0, etc.) conectado físicamente a una máquina en mi red, que por diferentes motivos (distancia, pereza, lo que sea), lo quería acceder con un programa en mi máquina, pero como si fuera local.

Es decir, tenía un programa en mi máquina que usa [pySerial](http://pyserial.sourceforge.net "pySerial") para acceder al Arduino en /dev/ttyACM0, pero por diferentes motivos el Arduino está conectado en otra máquina de mi red y quería que, sin tocar mi programa, éste acceda al Arduino como si estuviera directamente conectado a mi PC, haciendo de alguna manera "transparente" la red que nos separaba. Por suerte lo pude resolver, y quizás esta herramienta y acercamiento sirva a más de uno para resolver algún otro problema similar.

Para esto vamos a utilizar la herramienta [socat](http://www.dest-unreach.org/socat/ "Socat Home Page"), que es como [netcat](http://netcat.sourceforge.net/ "The GNU Netcat") pero para streams bidireccionales. Los pasos son:

- Instalar socat (apt-get install socat), tanto en la máquina que tiene el dispositivo serie real (vamos a llamarlo "servidor") como en el "cliente".
- Ejecutar en el servidor: "socat -d -d -d OPEN:/dev/ttyACM0 TCP4-LISTEN:31337":

```
usuario@servidor:~$ socat -d -d -d OPEN:/dev/ttyACM0 TCP4-LISTEN:31337
2013/05/27 10:40:53 socat[2703] I socat by Gerhard Rieger - see www.dest-unreach.org
2013/05/27 10:40:53 socat[2703] I This product includes software developed by the OpenSSL Project for use in the OpenSSL Toolkit. (http://www.openssl.org/)
2013/05/27 10:40:53 socat[2703] I This product includes software written by Tim Hudson (tjh@cryptsoft.com)
2013/05/27 10:40:53 socat[2703] N opening character device "/dev/ttyACM0" for reading and writing
2013/05/27 10:40:53 socat[2703] I open("/dev/ttyACM0", 02, 0666) -> 3
2013/05/27 10:40:53 socat[2703] I socket(2, 1, 6) -> 4
2013/05/27 10:40:53 socat[2703] I starting accept loop
2013/05/27 10:40:53 socat[2703] N listening on AF=2 0.0.0.0:31337
2013/05/27 10:40:58 socat[2703] I accept(4, {2, AF=2 192.168.1.150:44624}, 16) -> 5
2013/05/27 10:40:58 socat[2703] N accepting connection from AF=2 192.168.1.150:44624 on AF=2 192.168.1.93:31337
2013/05/27 10:40:58 socat[2703] I permitting connection from AF=2 192.168.1.150:44624
2013/05/27 10:40:58 socat[2703] I close(4)
2013/05/27 10:40:58 socat[2703] I resolved and opened all sock addresses
2013/05/27 10:40:58 socat[2703] N starting data transfer loop with FDs [3,3] and [5,5]
```

Esto hace que socat abra el archivo /dev/ttyACM0 (el dispositivo serie a compartir), y además abra un socket en modo LISTEN (escuchando por conexiones) en todas las interfaces del equipo, particularmente en el puerto 31337. Las opciones -d -d -d son opcionales y sirve para habilitar los mensajes de debug.

- Ejecutar en el cliente "socat -d -d -d TCP4:192.168.1.93:31337 PTY,link=/dev/ttyACM0":

```
usuario@cliente:~$ socat -d -d -d TCP4:192.168.1.93:31337 PTY,link=/tmp/ttyACM0
2013/05/27 11:52:34 socat[8408] I socat by Gerhard Rieger - see www.dest-unreach.org
2013/05/27 11:52:34 socat[8408] I This product includes software developed by the OpenSSL Project for use in the OpenSSL Toolkit. (http://www.openssl.org/)
2013/05/27 11:52:34 socat[8408] I This product includes software written by Tim Hudson (tjh@cryptsoft.com)
2013/05/27 11:52:34 socat[8408] N opening connection to AF=2 192.168.1.93:31337
2013/05/27 11:52:34 socat[8408] I starting connect loop
2013/05/27 11:52:34 socat[8408] I socket(2, 1, 6) -> 3
2013/05/27 11:52:34 socat[8408] N successfully connected from local address AF=2 192.168.1.150:45293
2013/05/27 11:52:34 socat[8408] I setting option "symbolic-link" to "/tmp/ttyACM0"
2013/05/27 11:52:34 socat[8408] I openpty({4}, {5}, {"/dev/pts/4"},,) -> 0
2013/05/27 11:52:34 socat[8408] N PTY is /dev/pts/4
2013/05/27 11:52:34 socat[8408] I resolved and opened all sock addresses
2013/05/27 11:52:34 socat[8408] N starting data transfer loop with FDs [3,3] and [4,4]
```

Este comando hace que el socat de mi máquina se conecte a la IP 192.168.1.93 (allí debe estar el servidor), al puerto 31337, y que localmente lo haga ver como un nuevo dispositivo serial (pseudo terminal) creado por socat mismo en /dev/pts/X, y además haga un symlink de éste al archivo /tmp/ttyACM0. A partir de éste momento en /tmp/ttyACM0 existe un archivo que puede ser abierto con pyserial (por ejemplo, puede ser minicom, etc.) y ser utilizado como un dispositivo serie local.

Un detalle, cuando se cierra la conexión del socat en el cliente hacia el servidor, en el servidor también se termina la ejecución del socat (y viceversa), por lo que en caso de volver a establecer la sesión, es necesario ejecutar nuevamente en ambos equipos.

Saludos
