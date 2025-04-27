---
author: mfernandez
category:
  - codear
  - linux
  - sysadmin
  - ubuntu-ar
date: "2009-01-04T19:45:00+00:00"
guid: https://blog.marcelofernandez.info/?p=123
title: Sobre KVM - Virtualización en Servidores
url: /2009/01/sobre-kvm-virtualizacion-en-servidores/

---
[![](http://4.bp.blogspot.com/_nDZ247g0qSM/SWEbCLcU0wI/AAAAAAAAB2E/gtbSu49pFeo/s400/banner_kvm_forum_2008.jpg)](http://kvm.qumranet.com/kvmwiki) Leyendo la blogósfera, me detuve en [esta crónica de una migración](http://blogs.thehumanjourney.net/oaubuntu/entry/about_kvm) de [VMWare Server](http://www.vmware.com/products/server/) a [KVM](http://kvm.qumranet.com/kvmwiki), explicando las razones de porqué elegir KVM y no el resto. :-)

Las enumero, tratando de traducirlas lo mejor posible:  

- El host corriendo KVM es un Linux estándar sin modificar. Para ambos ( [Xen](http://www.xen.org/) y [VMWare ESX](http://www.vmware.com/products/esxi/)) se necesita correr un Sistema Operativo modificado especialmente (que lo hace relativamente difícil de mantener al día con actualizaciones de seguridad).
- Siendo incluído en Linux, usted obtiene soporte de KVM en el mismo momento que usted compra soporte estándar de [Ubuntu Server](http://www.ubuntu.com/products/WhatIsUbuntu/serveredition). Como de todas maneras teníamos planeado adquirir [soporte Ubuntu](http://www.ubuntu.com/support/paid), tuvimos soporte para nuestra plataforma de virtualización sin costo adicional.
- Performance: La velocidad de KVM es muy buena, ya que está basada en extensiones de hardware. (N. del T.: Ojo, tengo entendido que tanto Xen como VMWare también soportan extensiones de hardware en caso de estar disponible).
- VMBuilder: Ubuntu desarrolló una herramienta (ubuntu-vm-builder, ahora llamada vmbuilder) que puede construir un appliance virtual en aproximadamente un minuto. Esta es una mejora enorme en comparación a la forma que solíamos crear VMs (VMbuilder también puede ahora crear imágenes para Xen y VMWare). (N. del T.: En mi experiencia, sirve únicamente si vamos a crear VMs con SO Ubuntu).  

- [KISS](http://en.wikipedia.org/wiki/KISS_principle) \- Keep It Simple, Stupid (Mantenlo Simple, Estúpido!): KVM utiliza todo lo que Linux provee de antemano. Cada VM es un simple proceso, usa su planificador de procesos (scheduler), uno puede reasignar la prioridad ( [renice](http://en.wikipedia.org/wiki/Renice)) del proceso, asignarlo a una CPU específica... esto resulta en una base de código fuente mucho más pequeña que Xen por ejemplo, de manera que asumo que será más fácil y barato mantenerlo a largo plazo, haciendo que el proyecto más confiable.
- Y lo más importante: Las companías involucradas. Aún cuando a KVM le faltan algunas características, los posts en la lista de desarrolladores muestran parches de código desde empresas como RedHat (que compró KVM el año pasado), AMD, Intel, IBM, HP, Novell, Bull... ¿A quién va a creerle, a un proyecto manejado por una única compañía (Citrix, VMWare), o a un proyecto soportado por tantos actores diferentes?  

Para mí le falta otra cosa importante: Tiene un desarrollo [totalmente abierto](http://kvm.qumranet.com/kvmwiki/Code) y es [software de cóodigo abierto](http://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine). :-)

En donde laburo estuvimos metiendo KVM en un servidor y si bien las herramientas de [configuración](https://help.ubuntu.com/8.04/serverguide/C/libvirt.html)/ [administración](http://virt-manager.et.redhat.com/index.html) tienen sus quirks (aunque funcionan relativamente bien), la parte de KVM en sí anda perfectamente y con una excelente performance y confiabilidad. ¡Ah! y Canonical [ya había anunciado](http://www.zdnet.com.au/news/software/soa/Ubuntu-bucks-trend-goes-for-KVM-virtualisation/0,130061733,339285828,00.htm?feed=pt_canonical) que iba a soportar KVM como plataforma principal de virtualización también, así que... IMHO, KVM tiene todas las de ganar de acá en adelante.

Espero postear próximamente un poco las experiencias que tuve armando VMs con este software, mientras tanto dejo [este enlace](https://help.ubuntu.com/community/KVM) que resume la mayoría de las cuestiones con KVM en Ubuntu.

Saludos  
Marcelo
