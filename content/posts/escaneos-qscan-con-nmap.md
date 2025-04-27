---
author: mfernandez
category:
  - codear
  - ubuntu-ar
  - web
date: "2007-09-29T22:56:00+00:00"
guid: https://blog.marcelofernandez.info/?p=85
title: Escaneos QScan con Nmap
url: /2007/09/escaneos-qscan-con-nmap/

---
Buenas... leyendo el [blog de Buanzo](http://blog.buanzo.com.ar/), me interesé por el tipo de ataques "QScan" que menciona en este artículo:

[http://blog.buanzo.com.ar/2007/09/informe-de-puertos-proxeados-fibertel.html](http://blog.buanzo.com.ar/2007/09/informe-de-puertos-proxeados-fibertel.html)

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/Rv7dRn7y11I/AAAAAAAAALo/WJdLioF0SlQ/s400/nmap_bnr_m4rc3l0-2.jpg)](http://insecure.org/nmap/)  


Con lo cual, después de una breve consulta en Google, me encontré con este excelente documento:

[http://hcsw.org/nmap/QSCAN](http://hcsw.org/nmap/QSCAN)

Básicamente, los tipos de escaneos QScan se basan en comparar las medias (mediante la [distribución T de Student](http://es.wikipedia.org/wiki/Distribuci%C3%B3n_t_de_Student)) de los [RTT](http://en.wikipedia.org/wiki/Round-trip_delay_time) s de una cantidad N (la muestra) de paquetes enviados a distintos puertos de un mismo (o diferentes) hosts. Además, para sacar más el jugo a la técnica, uno puede configurar el [intervalo de confianza](http://es.wikipedia.org/wiki/Intervalo_de_confianza), la cantidad de muestras y la demora N entre paquete y paquete enviado (que para hacer la prueba estadísticamente más 'fuerte', el parche hace un random que tiene como punto medio al N configurado). El objetivo es saber si el host que recibe los paquetes (el 'target' del escaneo) hace tratamientos diferentes de los paquetes de acuerdo a al puerto destino.

Por ejemplo, si un host tarda ~600 ms. en responder a los paquetes enviados por nmap al puerto 80 y ~250 ms. en responder a los paquetes enviados al puerto 22, y la comparación de las medias por T de Student da como resultado que son diferentes, se puede inferir que los paquetes enviados al puerto 80 son reenviados por el host a otro host (que no lo veo!), a diferencia de los paquetes que van al puerto 22.

Recomiendo muchísimo la lectura del artículo. Me parece un tipo de escaneo bastante útil para hacer detección de infraestructura de DMZs, reglas de firewall, etc. Hasta lo recomiendo porque es un tipo de escaneo muy sencillo e interesante por sus resultados.

Para los que tuvimos el gusto de asistir a sus clases (yo en la UNLu), es imposible no acordarse de Daniel Villa y sus clases de estadística leyendo este artículo! :-)

Lástima que este QScan no está disponible en la versión estándar de NMap (hay que recompilarlo).

Sitio oficial del parche: [http://www.hcsw.org/nmap/](http://hcsw.org/nmap/QSCAN)

Insisto, me alegré mucho personalmente por entender la aplicación de tanto sentido común (y con herramientas que uno conoce) sobre algo que no se me hubiera ocurrido. :-)

Saludos  
Marcelo
