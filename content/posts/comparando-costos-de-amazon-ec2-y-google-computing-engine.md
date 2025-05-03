---
author: Marcelo
category:
  - codear
  - linux
  - sysadmin
  - web
date: "2014-09-17T20:11:37+00:00"
guid: https://blog.marcelofernandez.info/?p=1394
title: Comparando costos de Amazon EC2 y Google Computing Engine
url: /2014/09/comparando-costos-de-amazon-ec2-y-google-computing-engine/

---
Estuve mirando y por suerte son prácticamente similares las tablas disponibles en cada site ( [EC2](http://aws.amazon.com/es/ec2/purchasing-options/dedicated-instances/ "Amazon EC2 Purchasing Options") / [GCE](https://cloud.google.com/products/compute-engine/ "Google Compute Engine")) y es relativamente sencillo compararlas \[1\]\[2\].

Para sus Data Centers en USA, establecí las siguientes relaciones:

- En las versiones de VMs " _standard_", los precios son exactamente iguales, con configuraciones _llamativamente_ similares.
- En las versiones " _high memory_", es más barato Google (la mitad), aunque Amazon te da el doble de CPUs por una VM con la misma cantidad de memoria.
  Ej: Google te da 4 CPUs / 26 GB de RAM y Amazon en cambio te da 8 CPUs para su configuración de 26 GB de RAM. Dado que estamos en " _high memory_", igualé a cantidad de memoria disponible para luego decir "Google es la mitad de barato".
- En las versiones " _large_" (" _high cpu_" de Google), Amazon es un 15% más caro para igual cantidad de CPUs, pero te da el doble de memoria en sus VMs.

**Observaciones**:

- Se desprende de lo anterior Amazon **EC2 ofrece perfiles más simétricos** **que Google** comparando la relación de cantidades de CPU/Memoria. Puede ser útil para algunas aplicaciones o no, ya que va obviamente asociado al costo.
- Amazon tiene configuraciones con más CPUs ya que llega a 32 CPUs y más memoria también: 244 GB; Google llega a 16 CPUs y 104 GB de memoria.
- No comparé tamaños de almacenamiento, asumo que no tiene tanta relevancia para nuestra aplicación (se disponen de varias decenas de GB en disco en ambos).
- Amazon dice que te incluye discos de estado sólido (SSD). Google te cobra ambos (?) 0,04 USD por GB/mes el disco estándar, 0,325 USD GB/mes el SSD.
- Fundamental para la región en la que vivo: **Amazon tiene Data Center en San Pablo (Brasil), Google no;** sólo sirve VMs desde USA, Europa y Asia/Pacífico.
- Amazon tiene diferentes tipo de instancias, en este caso comparé las "Bajo demanda" aka "Dedicadas" (según el lugar que se mire en la documentación), que son las más caras. Las otras son tipo prepagas ("Instancias Reservadas"), donde según entiendo, uno abona un importe fijo y después usa X cantidad de tiempo y se descuenta del fijo.
  Hay otros tipos de instancias pero no se ajustan a nuestro uso ("Instancias Puntuales" y "Instancias optimizadas para EBS"). En este apartado, pareciera que Amazon tiene largamente muchas más ofertas y más desarrollo del negocio que Google.

**Bonus Track: versus Linode.com**

- [Linode](https://www.linode.com/pricing "Linode - Pricing") tiene hasta 20 CPUs / 96 GB RAM máximo \[3\].
- [Linode](http://www.linode.com/ "Linode") es **mucho** más barato que ambos; es más simple su tabla de precios y más "1 a 1" la distribución de CPUs / RAM (4GB / 4CPUs, 8 GB / 6 CPUs, y así hacia arriba), además de que da bastante más storage en SSD.
  Por ejemplo, 4 GB / 4 CPUs en Linode: USD 0,06/hora (USD 40/mes), mientras que en Amazon estamos en USD 0,28/hora por 15GB / 4 CPUs o USD 0,105/hora por 3,75GB / 2 CPUs. Eso sí, Linode está en USA (Newark/Atlanta/Dallas/Fremont), Europa (Londres) y Asia (Tokio), no en Sudamérica.
- Comparando Linode contra GCE:
  - 1 Linode (2 GB de RAM/ 2 CPUs / 3 TB transfer / 48 GB SSD): U$S 20/mes.
  - 1 GCE (1,70 GB RAM / CPUs "shared" (?) / 48 GB SSD): U$S 33.29/mes (no dicen si hay límite de transferencia).
- A Linode lo conocemos los que estamos "en la pomada", y hay casos en donde suena mucho mejor "lo tengo repartido en la nube de Amazon/Google", nobleza obliga.

\[1\] [http://aws.amazon.com/es/ec2/purchasing-options/dedicated-instances/](http://aws.amazon.com/es/ec2/purchasing-options/dedicated-instances/ "Amazon EC2 Purchasing Options")
\[2\] [https://cloud.google.com/products/compute-engine/](https://cloud.google.com/products/compute-engine/ "Google Compute Engine")\[3\] [https://www.linode.com/pricing](https://www.linode.com/pricing "Linode - Pricing")

¿Sugerencias, comentarios?

Saludos
