---
author: mfernandez
category:
  - codear
  - programación
  - python
  - ubuntu-ar
date: "2009-07-25T18:11:41+00:00"
guid: https://blog.marcelofernandez.info/?p=346
title: 'Control de Versiones: Manejando las diferencias entre distribuido y centralizado'
url: /2009/07/control-de-versiones-manejando-las-diferencias-entre-distribuido-y-centralizado/

---
![VCSs](/wp-content/uploads/2009/07/VCSs.png)Dando vueltas [por ahí](http://wxwidgets.blogspot.com/2009/07/playing-with-dvcs-for-wxwidgets.html), me entero que es muy sencillo utilizar localmente versionado de código fuente (gracias a los sistemas de control de versiones distribuidos, como por ejemplo [Mercurial](http://mercurial.selenic.com/wiki/), [GIT](http://git-scm.com/) y [Bazaar](http://bazaar-vcs.org/)) para luego subir/actualizar los cambios a un repositorio [Subversion](http://subversion.tigris.org/) principal. Es decir, en vez de usar los comandos svn como clientes de un servidor Subversion, es posible utilizar alguno de estos sistemas distribuidos como clientes "consumidores" del repositorio principal, para luego aprovechar [sus importantes ventajas](http://en.wikipedia.org/wiki/Distributed_revision_control#Advantages) en forma local.

En principio, admito que no conozco ningún [VCS](http://en.wikipedia.org/wiki/Revision_control) distribuido, más que nada por vagancia (je), pero esto en particular sí lo necesito de vez en cuando, principalmente cuando no estoy conectado, y/o cuando los cambios que hago con respecto a _trunk_ son profundos, llevan tiempo y quiero tener un control más "fino" en lo que hago.

A ver, habiendo nombrado primero los sistemas más conocidos (por mí), se desprende que tenemos:

- [![Logo Mercurial](/wp-content/uploads/2009/07/logo-droplets-200.png)](http://mercurial.selenic.com/wiki/)[Mercurial-Svn](http://bitbucket.org/durin42/hgsubversion/wiki/Home): Si bien el autor del artículo que me inspiró a escribir esto ( [Vadim Zeitlin](http://www.linkedin.com/pub/vadim-zeitlin/6/145/445), desarrollador de [wxWidgets](http://www.wxwidgets.org/)) comentaba que le gusta muchísimo Mercurial por su fácil uso, al momento de probar esta extensión para subir los cambios al servidor [SVN de wxWidgets](http://svn.wxwidgets.org) tuvo problemas de performance, uso de memoria, etc. De todas maneras, el código fuente en el repositorio de wxWidgets [es bastante grandote](http://trac.wxwidgets.org/browser) (~330MB), así que quizás funcione bien para proyectos chicos/medianos, y más si uno ya conoce Mercurial.

- [Git-Svn](http://www.kernel.org/pub/software/scm/git/docs/git-svn.html): Bueno, citando nuevamente a Vadim, parece que critica un poco lo complicado de los comandos Git, sin embargo termina recomendándolo en forma rotunda por su velocidad y poca utilización de espacio extra en el disco\*. [Acá](http://flavio.castelli.name/howto_use_git_with_svn) parece haber un tutorial cortito para utilizarlo (Google me [devuelve](http://utsl.gen.nz/talks/git-svn/intro.html) [muchísimos](http://tsunanet.blogspot.com/2007/07/learning-git-svn-in-5min.html) [enlaces](http://git.or.cz/course/svn.html) [más](http://xmleye.wordpress.com/2008/10/13/recuperando-repo-svn-desde-clon-git-svn/) por si es poco).

[![Git Version Control](/wp-content/uploads/2009/07/header.gif)](http://git-scm.com/)

- [Bazaar-Svn](http://bazaar-vcs.org/BzrForeignBranches/Subversion): A [Bazaar](http://bazaar-vcs.org/) lo conozco por su relación estrecha con [Launchpad](http://launchpad.net) y [Ubuntu](http://www.ubuntu.com), por lo tanto yo probaría a este primero... ah, y ellos mismos elaboraron una comparativa (imparcial) entre [Bazaar y Git](http://bazaar-vcs.org/BzrVsGit) por un lado, y entre [Bazaar y Mercurial](http://bazaar-vcs.org/BzrVsHg) por el otro.

En fin, recién me sumerjo en la lectura y experimentación de todo esto, pero para aquellos que trabajan contra un servidor SVN\*\* y suben sus cambios con poca frecuencia al repositorio principal, "rompiendo todo" cada vez que lo hacen (guiño, guiño), me parece que esto les viene perfecto.

[![Bazaar](/wp-content/uploads/2009/07/Bazaar1.png)](http://bazaar-vcs.org) Además, tengo entendido que no es difícil usar Bazaar (por poner uno de ejemplo), y realmente está muy _cool_ poder volver atrás en los cambios que uno hizo desde que hizo el _checkout_/ _update_ del SVN, mientras desarrolla, y sin tener que conectarse a Internet (VPN/SSH/etc.).

(\*) Cuando el cliente de control de versiones distribuido hace un _checkout_ del repositorio Subversion, no sólo descarga la última versión de _trunk_, sino que descarga **todas las versiones pasadas de todas las ramas existentes**, con lo cual el consumo de espacio en disco local pasa a ser un asunto importante. Git, según Vadim, parece tener poco _overhead_ en este sentido comparado con la extensión de Mercurial.

(\*\*) Una de las cuestiones que no le hacen perder valor e importancia a SVN es que, al ser centralizado, sigue siendo relevante en empresas que quieren tener un control más o menos estricto de su código; por lo tanto, la migración a sistemas distribuidos no siempre estará bajo su radar. Es por eso que hablo de "diferencias" en el título del post y no de una "futura migración", es decir, **que los distribuidos sean los "nuevos vecinos de la cuadra" no quiere decir que vengan a reemplazar a los viejos y conocidos**. :-)

¡Saludos!
