---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2008-10-23T17:59:00+00:00"
guid: https://blog.marcelofernandez.info/?p=113
title: Tip para vagos (como yo) al usar SSH
url: /2008/10/tip-para-vagos-como-yo-al-usar-ssh/

---
Tip estilo "cortita y al pie".

Para el que tiene varios equipos remotos a los que entra usualmente por SSH pero bajo diferentes usuarios al del host propio, termina siendo un garrón tener que escribir diferentes usuarios, y un montón de veces, "ssh usuariopepe@mihost".

Pero con sólo crear un archivo .ssh/config parecido a este:

```
marcelo@marcelo-laptop:~$ cat .ssh/config
Host *.dominio.com.ar *.dominio server1 server2 server3
User mfernandez

Host desarrollo.dominio.com.ar desarrollo.dominio desarrollo
User usuario1

Host vm1 vm2
User prueba
```

A partir de ahora sólo hay que escribir "ssh vm1" para entrar!!!

Más info en "man ssh\_config". :-)

Saludos
Marcelo
