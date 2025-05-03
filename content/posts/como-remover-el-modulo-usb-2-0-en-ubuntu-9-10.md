---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2010-03-19T18:14:13+00:00"
guid: https://blog.marcelofernandez.info/?p=553
title: Cómo remover el módulo USB 2.0 en Ubuntu 9.10+
url: /2010/03/como-remover-el-modulo-usb-2-0-en-ubuntu-9-10/

---
Este post va a intentar ser breve y me parece que es bastante técnico, pero puede serle útil a más de uno.

[![](/wp-content/uploads/2010/03/SCX4200-300x236.jpg)](/wp-content/uploads/2010/03/SCX4200.jpg) Resulta que yo tengo una Impresora Multifunción [Samsung SCX-4200](http://www.samsung.com/ar/support/download/supportDown.do?group=&group_cd=&type=impresoras&type_cd=06010000&subtype=multifunci%C3%B3nblancoynegro&subtype_cd=06010300&model_nm=SCX-4200&dType=D&mType=&model_cd=&menu=&prd_ia_cd=06010300&disp_nm=SCX-4200&language=), que tiene unos drivers (cerrados) para Linux \[1\]. Estos drivers sirven para imprimir y escanear, y si bien hoy en día Ubuntu se las arregla bárbaro para imprimir con los drivers libres de [SpliX](http://splix.ap2c.org/) (¡instalación y configuración automágica!), queda pendiente aún el tema del escaneo.

Por algún motivo al utilizar el scanner (vía USB) la transferencia se caía, y resultó ser que la comunicación Scanner <-> USB 2.0 no funciona, como [ya documenté en este mismo blog](/2007/02/samsung-scx-4200-en-ubuntu-610/) hace unos años (con la impresión no hay problemas); y no importa la mother, chip o cosa que haya en el medio (lo probé con diferentes equipos). O es esta printer en particular (mucha mala suerte), o es algo del firmware que me tocó en gracia, o... _shit happens_. :-(

En fin, en versiones anteriores de Ubuntu anteriores a 9.10, **para forzar una comunicación USB 1.1 se solucionaba con un "sudo rmmod ehci\_hcd"**, porque el módulo USB 2.0 (ehci\_hcd) era justamente, un módulo que se cargaba dinámicamente. Pero a partir de Ubuntu 9.10 (¿o antes?) este módulo fue incluido en el kernel (estáticamente), con lo cual a priori no tenía manera sencilla de forzar una comunicación USB 1.1 con algún dispositivo (que no sea cambiar el parámetro correspondiente en el BIOS), ya que no es posible descargar un módulo estático del kernel (más info [acá](http://structio.sourceforge.net/guias/AA_Linux_colegio/kernel-y-modulos.html)).

[![](/wp-content/uploads/2010/03/800px-USB_types_2-300x139.jpg)](/wp-content/uploads/2010/03/800px-USB_types_2.jpg)
Me puse a buscar, y encontré una solución. Para simular la "descarga" el módulo es necesario posicionarse en el directorio /sys/bus/pci/drivers/ehci\_hcd y pasarle al archivo "unbind" qué hub USB "unbindear" (¿"desrelacionar"?); los hubs USB se pueden ver con un lsusb -v, y los que van a aparecer en el directorio mencionado son los de "Full Speed", o sea, los de USB 2.0 \[2\] .

Si tu PC soporta USB 2.0 (y por ende tenés este problema, je), al menos uno hay, que corresponde al Hub de la motherboard. En mi caso hay dos, pero para no andar especulando (y porque quiero que el scanner funcione sin averiguar a cuál de los dos hub está conectado en particular), "unbindeo"/"desrelaciono"/"desenlazo" (?) ambos hubs:

```
marcelo@marcelo-laptop:~$ sudo -i
[sudo] password for marcelo:
root@marcelo-laptop:~# cd /sys/bus/pci/drivers/ehci_hcd/
root@marcelo-laptop:/sys/bus/pci/drivers/ehci_hcd# ls -l
total 0
lrwxrwxrwx 1 root root    0 2010-03-19 14:12 0000:00:1a.7 -> ../../../../devices/pci0000:00/0000:00:1a.7
lrwxrwxrwx 1 root root    0 2010-03-19 14:12 0000:00:1d.7 -> ../../../../devices/pci0000:00/0000:00:1d.7
--w------- 1 root root 4096 2010-03-19 14:12 bind
lrwxrwxrwx 1 root root    0 2010-03-19 14:12 module -> ../../../../module/ehci_hcd
--w------- 1 root root 4096 2010-03-19 14:12 new_id
--w------- 1 root root 4096 2010-03-19 14:12 remove_id
--w------- 1 root root 4096 2010-03-19 14:12 uevent
--w------- 1 root root 4096 2010-03-19 14:12 unbind
root@marcelo-laptop:/sys/bus/pci/drivers/ehci_hcd# echo -n 0000\:00\:1a.7 > unbind
root@marcelo-laptop:/sys/bus/pci/drivers/ehci_hcd# echo -n 0000\:00\:1d.7 > unbind
root@marcelo-laptop:/sys/bus/pci/drivers/ehci_hcd# ls -l
total 0
--w------- 1 root root 4096 2010-03-19 14:12 bind
lrwxrwxrwx 1 root root    0 2010-03-19 14:12 module -> ../../../../module/ehci_hcd
--w------- 1 root root 4096 2010-03-19 14:12 new_id
--w------- 1 root root 4096 2010-03-19 14:12 remove_id
--w------- 1 root root 4096 2010-03-19 14:12 uevent
--w------- 1 root root 4096 2010-03-19 14:21 unbind
root@marcelo-laptop:/sys/bus/pci/drivers/ehci_hcd#
```

Listo el pollo. Todos los dispositivos USB que estén conectados por medio de estos Hubs van a ser reconocidos y utilizados por el "fallback", USB 1.1; en definitiva, este es manejado por el módulo linux uhci\_hcd, como podemos ver con el comando dmesg:

```
[...]
[64720.180444] ehci_hcd 0000:00:1a.7: remove, state 4
[64720.180464] usb usb1: USB disconnect, address 1
[64720.184731] ehci_hcd 0000:00:1a.7: USB bus 1 deregistered
[64720.184838] ehci_hcd 0000:00:1a.7: PCI INT C disabled
[64730.610396] ehci_hcd 0000:00:1d.7: remove, state 1
[64730.610417] usb usb2: USB disconnect, address 1
[64730.610422] usb 2-4: USB disconnect, address 3
[64730.684860] ehci_hcd 0000:00:1d.7: USB bus 2 deregistered
[64730.684980] ehci_hcd 0000:00:1d.7: PCI INT A disabled
[64731.070210] usb 6-2: new full speed USB device using uhci_hcd and address 5
[64731.254158] usb 6-2: configuration #1 chosen from 1 choice
[64731.259814] uvcvideo: Found UVC 1.00 device FO13FF-50 PC-CAM (05c8:0100)
[64731.273207] input: FO13FF-50 PC-CAM as /devices/pci0000:00/0000:00:1d.1/usb6/6-2/6-2:1.0/input/input11
[...]
```

En mi caso no tenía conectada la impresora, pero sirve el ejemplo de todas maneras ver cómo la webcam es reconocida por uhci\_hcd (USB 1.1). Para volver atrás, supongo que hay que hacer lo mismo pero con bind (no lo probé, sólo me parece lógico...), aunque yo reinicio la máquina para no complicarme. :-P

Esto es fácil de meter en un script, y espero que le sirva a alguien (me llevó un buen rato encontrarlo).

Saludos

Marcelo

\[1\] Por algún motivo la gente de Samsung sacó los drivers Linux de la página de la printer al momento de escribir esto (?). Ya les envié un mail al respecto, espero que contestesten y/o vuelvan a aparecer (habían sido actualizados hace un par de meses, no parecían estar sin mantener...). Si alguien los necesita, me escribe y los pongo en algún lugar accesible.

Actualización: Me contestaron enseguida, a las 3 horas de haberles escrito que no hay drivers Linux en su página. Según ellos, se disculpan y afirman que puede que el equipo de desarrollo esté actualizando los drivers en este mismo momento... hay que ver en unos días si aparecen.

\[2\] Acá hay una porción de la salida de un "lsusb -v" donde aparece uno de los 2 Root Hub USB 2.0 que tengo en mi equipo; ver que el archivo relacionado que aparece en el directorio /sys/bus/pci/drivers/ehci\_hcd corresponde al descriptor "iSerial" del mismo:

```
[...]
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            9 Hub
  bDeviceSubClass         0 Unused
  bDeviceProtocol         0 Full speed (or root) hub
  bMaxPacketSize0        64
  idVendor           0x1d6b Linux Foundation
  idProduct          0x0002 2.0 root hub
  bcdDevice            2.06
  iManufacturer           3 Linux 2.6.31-20-generic ehci_hcd
  iProduct                2 EHCI Host Controller
  iSerial                 1 0000:00:1d.7
  bNumConfigurations      1
[...]
```
