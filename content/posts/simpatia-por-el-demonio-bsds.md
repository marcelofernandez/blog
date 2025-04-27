---
author: mfernandez
category:
  - codear
  - tests
  - ubuntu-ar
date: "2007-12-31T16:20:00+00:00"
guid: https://blog.marcelofernandez.info/?p=94
title: 'Simpatía por el Demonio: BSDs'
url: /2007/12/simpatia-por-el-demonio-bsds/

---
Hola gente!

Aprovechando ayer que tuve "libre" mi día, me puse a instalar los dos SOs [BSDs](http://en.wikipedia.org/wiki/Bsd#Significant_BSD_descendants) libres que me interesan (aunque por motivos diferentes): [OpenBSD 4.2](http://www.openbsd.org/) y [FreeBSD 6.2](http://www.freebsd.org/) (para AMD64), cada uno adentro de una máquina virtual de [VMWare](http://www.vmware.com/).

Por si alguien no los conocía, estos sistemas operativos no solamente son "Unix-Like" (es decir, del tipo Unix) como Linux, sino que son Unix; quiero decir, que los BSD son "hijos" del [árbol del Unix original](http://en.wikipedia.org/wiki/Unix), y conservan muy fuertemente sus raíces y filosofía. Además de eso, son Software Libre, están publicados con una [licencia mucho menos restrictiva](http://opensource.org/licenses/bsd-license.php) que [la de Linux](http://opensource.org/licenses/gpl-license.php), y son gratis!

[![](http://1.bp.blogspot.com/_nDZ247g0qSM/R3kZA3X-JMI/AAAAAAAAAOs/3Z8y9ErCgXM/s400/logo-red.png)](http://www.freebsd.org/)  


La instalación de ambos es MUY sencilla y sorprendentemente corta; no requiere conocimientos técnicos ni del SO muy profundos, aunque es bueno tener el navegador a mano para ir chusmeando en los manuales de instalación\[1\],\[2\] porque alguna duda siempre surge.

[![](http://2.bp.blogspot.com/_nDZ247g0qSM/R3kblHX-JQI/AAAAAAAAAPM/qKXjzkGK8rE/s400/wanthead2.gif)](http://www.openbsd.org/)  


Además, aunque suma que no tuve que hacerme problema por particiones, al asignar todo el "disco" al SO, también hubiera sido sencilla igual, una vez que uno conoce que los BSDs manejan una "partición" Linux/Windows como un único "volumen" (en BSD se denominan "slices"), que adentro tienen varias "particiones". De esa manera, uno siempre "ve" desde Linux/Windows una única partición en el disco. Acá se puede ver gráficamente el concepto\[3\]:  

[![](http://4.bp.blogspot.com/_nDZ247g0qSM/R3kaznX-JPI/AAAAAAAAAPE/f-hYbBy8900/s400/disk-layout.png)](http://4.bp.blogspot.com/_nDZ247g0qSM/R3kaznX-JPI/AAAAAAAAAPE/f-hYbBy8900/s1600-h/disk-layout.png)  

Eso sí, es todo el proceso se realiza modo texto y la de FreeBSD es más bien un "asistente", me hizo recordar al viejo instalador de Debian (que se puede ver por ejemplo cuando uno instala un Ubuntu Alternate CD).

La [documentación](http://www.freebsd.org/docs.html) y [soporte](http://www.freebsd.org/community.html) es abundante y excelente en FreeBSD: el Handbook multilenguaje (tiene una calidad impresionante) se combina con un extenso soporte comunitario, en varios lenguajes y y formatos, casos de uso, muchos sitios "extras" dedicados a brindar información; si bien esto es "común" en las distribuciones de Linux más grandes, es algo a destacar, ya que uno no está del todo perdido cuando necesita una mano, y es algo que agradezco cada vez que tengo un problema con el pingüino. El sitio oficial es muy cómodo, y nuevamente, multilenguaje.

En OpenBSD no tienen "handbook": consideran a los "man" como documentación oficial, y además tienen un FAQ; más allá de esto, tienen [listas de correo](http://www.openbsd.org/mail.html) en unos cuantos lenguajes.

En cuanto a soporte de hardware, creo que por popularidad (y por tener algunos drivers desarrollados bajo [NDA](http://es.wikipedia.org/wiki/NDA)), FreeBSD tiene más soporte que OpenBSD (cuyo pilar fundamental es ser lo más libre posible). Aún así, estimo que ambos soportan menos hardware que Linux, aunque para el uso como servidor (mi objetivo) no debería ser un problema, ya que el déficit está por lo general en chips "baratos" usualmente en las PCs de escritorio.

Al utilizarlos (sólo por consola, no instalé X en ninguno), no necesité las vmware-tools, ya que la interfaz de red es detectada automáticamente como placa intel (interfaz 'em0'). Sin embargo, es bueno destacar que si queremos aprovechar la totalidad de los dispositivos virtuales de vmware, en ambos se pueden instalar, ya que VMWare provee las vmware-tools para FreeBSD y éstas mismas se pueden instalar para OpenBSD ( [más info acá)](http://www.openbsd-wiki.org/index.php?title=HowTo_install_VMWare_tools).

Instalar paquetes es muy sencillo y similar en ambos: los comandos pkg\_\* te ayudan para instalar los paquetes binarios de los repositorios. FreeBSD trae de "fábrica" el shell 'sh' y el OpenBSD trae el Korn Shell para root ('sh' para el resto), así que para instalar bash (más dependencias) fue tanto como ejecutar "pkg-add -r bash" en FreeBSD y "pkg-add bash" en OpenBSD (previo hacer un "export PKG\_PATH=ftp..." para señalar nuestro repositorio).

También existen los ports, que es un mecanismo que descarga, compila e instala paquetes desde código fuente (siempre desde el repositorio del proyecto o alguno de sus mirrors), usualmente más actualizado que los paquetes binarios.

OpenBSD me interesa con el objetivo de ser Firewall, IDS, Proxy, etc., ya que me pareció muy minimalista, liviano, simple y tiene un claro objetivo de ser seguro. Como desventaja, tiene menos popularidad que su hermano FreeBSD, menos documentación y es algo más lento (ya que su objetivo es otro, ser seguro por defecto).

FreeBSD me da la impresión que es más bien un reemplazo de un servidor Linux (pero de servicios clásicos típicos Unix, como ser Mail, FTP, Web), ya que lo utiliza mucha más gente (una búsqueda por foros, enlaces, etc. da más importancia a FreeBSD) , y quizás por esto está bastante más elaborado que OpenBSD: da la impresión de ser menos "minimalista" y un poco más amigable.

Entre las ventajas de ambos con respecto a Linux es que son sistemas operativos completos, no un conjunto de utilidades que posteriormente son empaquetadas en un CD. Esto quiere decir que cada paquete o port que uno instala está revisado e integrado por la comunidad del proyecto.

Otra ventaja interesante que vale la pena destacar es que con ambos se puede utilizar [PF](http://www.openbsd.org/faq/pf/index.html) (Packet Filter), que es un filtro de paquetes de red super-poderoso y sencillo de utilizar (al menos más sencillo que [Netfilter](http://www.netfilter.org/), el que está incluído en Linux). De hecho, en FreeBSD hay 3 módulos para filtrado de paquetes, aunque PF es el más conocido (al menos por mí, je). Los comandos son parecidos (algunos) y la mayoría iguales a los de Linux.

Personalmente me encuentro cómodo con sus consolas, y es cierto, se sienten más "coherentes" o "prolijas" las cosas, la documentación es clara... aparte, hay detalles que están buenos: por ejemplo, al hacerle un escaneo nmap al FreeBSD, automáticamente hizo un 'throttle' de los RST que devolvía al host que escaneaba, aumentando el delay. Y eso sin usar PF. Groso. (aunque nmap se dió cuenta y prolongó el delay, pero bueno, son detalles que valen la pena).

La única duda que tengo es en cuanto a las actualizaciones, es decir, cuando sale una nueva release.. qué hago? (sin volver a instalar, claro está). :-)

Como yapa, me encuentro que dentro de los ["artículos" en español](http://docs.freebsd.org/doc/6.2-RELEASE/usr/share/doc/es/articles/), está el de un conocido, la migración de [argentina.com](http://docs.freebsd.org/doc/6.2-RELEASE/usr/share/doc/es/articles/casestudy-argentina.com/) a FreeBSD.

Bueno, espero haber dado un pantallazo (con este calorrr) a estos SOs que siempre quise testear, sólo estuve un día y medio dando vueltas con ellos, así que no me "peguen" si le pifié u olvidé algo, comenten y en todo caso con más experiencia armo otro post más adelante.

Espero que pronto les pueda dar utilidad a alguno de los dos en alguna de mis actividades, ya que creo que en la heterogeneidad está la seguridad y el conocimiento, a fin y al cabo.

Feliz 2008 a todos!  
Marcelo

Links:  
\[1\]: FreeBSD Handbook:  
Inglés: [http://www.freebsd.org/doc/en/books/handbook/index.html (actualizado 2007)](http://www.freebsd.org/doc/en/books/handbook/index.html)  
Español: [http://www.freebsd.org/doc/es/books/handbook/index.html (actualizado 2005)](http://www.freebsd.org/doc/es/books/handbook/index.html)

\[2\]: OpenBSD FAQ:  
[http://www.openbsd.org/faq/index.html](http://www.openbsd.org/faq/index.html)

\[3\]: [http://www.freebsd.org/doc/en/books/handbook/disk-organization.html \[...\]](http://www.blogger.com/%5B1%5D:http://www.freebsd.org/doc/en/books/handbook/disk-organization.html#BASICS-DISK-SLICE-PART)
