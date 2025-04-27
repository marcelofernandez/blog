---
author: mfernandez
category:
  - codear
  - linux
  - programación
  - ubuntu-ar
date: "2010-06-20T11:00:18+00:00"
guid: https://blog.marcelofernandez.info/?p=898
title: Free Pascal y Lazarus en Ubuntu
url: /2010/06/free-pascal-y-lazarus-en-ubuntu/

---
Creo que la gran mayoría de los programadores debemos recordar aquellos primeros momentos en que uno pasaba tardes y noches enteras escribiendo y escribiendo líneas de código en aquellas XT, AT, Commodores 64/128, etc., sólo por amor al arte y puro fanatismo. Supongo que diríamos lo mismo de los primeros años de facultad, cuando en materias de título "Programación I" uno repasaba el paradigma de la [Programación Estructurada](http://es.wikipedia.org/wiki/Programaci%C3%B3n_estructurada), casi siempre en el lenguaje de aprendizaje facultativo por excelencia, al menos acá en Argentina: **[Pascal](http://es.wikipedia.org/wiki/Lenguaje_de_programaci%C3%B3n_Pascal)**, usando [Turbo Pascal](http://en.wikipedia.org/wiki/Turbo_pascal), su versión más popular.

Viniendo más acá en el tiempo, y buscando alguna herramienta donde aprovechar mis conocimientos de este lenguaje pero para desarrollar sobre Linux, supe de la existencia de [Free Pascal](http://freepascal.org/), un compilador Pascal libre, multiplataforma y con soporte a diferentes arquitecturas, que genera ejecutables binarios con dependencias mínimas. También estaba disponible [Lazarus](http://www.lazarus.freepascal.org/), su inseparable IDE, no casualmente muy parecido a [Delphi](http://en.wikipedia.org/wiki/CodeGear_Delphi).

El problema era que hasta Ubuntu 9.04 y por una cuestión de madurez, las bibliotecas gráficas de Lazarus aún dependían de GTK 1.2 ( completamente en desuso desde hace "siglos" en las distribuciones Linux), y se veía así:

[![](/wp-content/uploads/2010/06/Lazarus_IDE_GTK1_Linux-300x225.png)](/wp-content/uploads/2010/06/Lazarus_IDE_GTK1_Linux.png)

Se veía "duro", anticuado y lejos de toda amigabilidad con el usuario programador, carente de características básicas hoy en día tales como [Unicode](http://es.wikipedia.org/wiki/Unicode), [antialiasing](http://es.wikipedia.org/wiki/Antialiasing) y mucho más... por lo tanto, concluí en ese momento que uno no podía encarar ningún desarrollo nuevo para Linux/Unix usando GTK 1.2 (y por ende Free Pascal+Lazarus), sucedido por la versión 2.0 allá por el año 2002.

En estos días leí un mail en la lista de [Ubuntu-Ar](http://ubuntu-ar.org/) sobre Lazarus y Free Pascal, y decidí a instalarlo para echarle un vistazo otra vez. Por suerte la situación cambió bastante, me encontré con un bonito e importante cambio:

[![](/wp-content/uploads/2010/06/Pantallazo1-300x187.png)](/wp-content/uploads/2010/06/Pantallazo1.png)

Ahora, con Lazarus sobre GTK 2, ¡sí que está mucho más decente! Junto a [Gambas](http://es.wikipedia.org/wiki/Gambas) es lo más parecido a un entorno [RAD](http://es.wikipedia.org/wiki/Desarrollo_r%C3%A1pido_de_aplicaciones) "estilo Visual Basic" que hay dando vueltas para Linux. Y es aún mejor que éstos dos:

- Libre: [Licencia LGPL](http://freepascal.org/faq.var#general-license) (salvo el compilador que es GPL). Permite desarrollar software con cualquier licencia, incluso comercial.
- Gratuito: Viene con los fuentes de sí mismo ¡y se [compila a sí mismo](http://en.wikipedia.org/wiki/Self-hosting)!
- Multiplataforma: DOS, Linux, Windows, Mac OSX, FreeBSD, Solaris, y [más](http://freepascal.org/docs-html/user/userse2.html#x5-40001.2).
- Soporte de diferentes arquitecturas: Intel, AMD64, ARM, PowerPC, Sparc...
- Diferentes Toolkits GUI: Win32/Win64, GTK1/2, QT3/4, Carbon, Cocoa, (aunque en diferentes estados de madurez).
- Compatibilidad en [un alto porcentaje](http://wiki.lazarus.freepascal.org/Lazarus_For_Delphi_Users/es) con Object Pascal y Delphi.

Por último, me hice un "Hola Mundo" muy muy sencillo y lo compilé. Luego, ejecuté el comando [ldd](http://linux.about.com/library/cmd/blcmdl1_ldd.htm), para saber de qué bibliotecas dependía. El binario, que ocupaba 14 MB (ojo, era todo info de debug, luego de pasarle un [strip](http://www.computerhope.com/unix/strip.htm) ocupó 5.1 MB) dependía de esto:

```
marcelo@marcelo-laptop:/tmp$ ldd project1
	linux-vdso.so.1 =>  (0x00007fff441ff000)
	libX11.so.6 => /usr/lib/libX11.so.6 (0x00007f6603e2f000)
	libgdk_pixbuf-2.0.so.0 =>; /usr/lib/libgdk_pixbuf-2.0.so.0 (0x00007f6603c13000)
	libgtk-x11-2.0.so.0 => /usr/lib/libgtk-x11-2.0.so.0 (0x00007f66035f0000)
	libgdk-x11-2.0.so.0 => /usr/lib/libgdk-x11-2.0.so.0 (0x00007f6603343000)
	libgobject-2.0.so.0 => /usr/lib/libgobject-2.0.so.0 (0x00007f66030fb000)
	libglib-2.0.so.0 => /lib/libglib-2.0.so.0 (0x00007f6602e1c000)
	libgthread-2.0.so.0 => /usr/lib/libgthread-2.0.so.0 (0x00007f6602c17000)
	libgmodule-2.0.so.0 => /usr/lib/libgmodule-2.0.so.0 (0x00007f6602a13000)
	libpango-1.0.so.0 => /usr/lib/libpango-1.0.so.0 (0x00007f66027c8000)
	libpthread.so.0 => /lib/libpthread.so.0 (0x00007f66025ab000)
	libatk-1.0.so.0 => /usr/lib/libatk-1.0.so.0 (0x00007f660238a000)
	libcairo.so.2 => /usr/lib/libcairo.so.2 (0x00007f6602106000)
	libdl.so.2 => /lib/libdl.so.2 (0x00007f6601f02000)
	libc.so.6 => /lib/libc.so.6 (0x00007f6601b80000)
	libxcb.so.1 => /usr/lib/libxcb.so.1 (0x00007f6601963000)
	libgio-2.0.so.0 => /usr/lib/libgio-2.0.so.0 (0x00007f66016b0000)
	librt.so.1 => /lib/librt.so.1 (0x00007f66014a8000)
	libm.so.6 => /lib/libm.so.6 (0x00007f6601224000)
	libXext.so.6 => /usr/lib/libXext.so.6 (0x00007f6601012000)
	libXrender.so.1 => /usr/lib/libXrender.so.1 (0x00007f6600e08000)
	libXinerama.so.1 => /usr/lib/libXinerama.so.1 (0x00007f6600c04000)
	libXi.so.6 => /usr/lib/libXi.so.6 (0x00007f66009f4000)
	libXrandr.so.2 => /usr/lib/libXrandr.so.2 (0x00007f66007eb000)
	libXcursor.so.1 => /usr/lib/libXcursor.so.1 (0x00007f66005e0000)
	libpangocairo-1.0.so.0 => /usr/lib/libpangocairo-1.0.so.0 (0x00007f66003d3000)
	libXcomposite.so.1 => /usr/lib/libXcomposite.so.1 (0x00007f66001d0000)
	libXdamage.so.1 => /usr/lib/libXdamage.so.1 (0x00007f65fffcc000)
	libXfixes.so.3 => /usr/lib/libXfixes.so.3 (0x00007f65ffdc6000)
	libpangoft2-1.0.so.0 => /usr/lib/libpangoft2-1.0.so.0 (0x00007f65ffb9c000)
	libfreetype.so.6 => /usr/lib/libfreetype.so.6 (0x00007f65ff915000)
	libz.so.1 => /lib/libz.so.1 (0x00007f65ff6fe000)
	libfontconfig.so.1 => /usr/lib/libfontconfig.so.1 (0x00007f65ff4c9000)
	libpcre.so.3 => /lib/libpcre.so.3 (0x00007f65ff29a000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f6604184000)
	libpixman-1.so.0 => /usr/lib/libpixman-1.so.0 (0x00007f65ff041000)
	libdirectfb-1.2.so.0 => /usr/lib/libdirectfb-1.2.so.0 (0x00007f65fedbd000)
	libfusion-1.2.so.0 => /usr/lib/libfusion-1.2.so.0 (0x00007f65febb3000)
	libdirect-1.2.so.0 => /usr/lib/libdirect-1.2.so.0 (0x00007f65fe99a000)
	libpng12.so.0 => /lib/libpng12.so.0 (0x00007f65fe772000)
	libxcb-render-util.so.0 => /usr/lib/libxcb-render-util.so.0 (0x00007f65fe56e000)
	libxcb-render.so.0 => /usr/lib/libxcb-render.so.0 (0x00007f65fe365000)
	libXau.so.6 => /usr/lib/libXau.so.6 (0x00007f65fe160000)
	libXdmcp.so.6 => /usr/lib/libXdmcp.so.6 (0x00007f65fdf5a000)
	libresolv.so.2 => /lib/libresolv.so.2 (0x00007f65fdd41000)
	libselinux.so.1 => /lib/libselinux.so.1 (0x00007f65fdb22000)
	libexpat.so.1 => /lib/libexpat.so.1 (0x00007f65fd8f9000)
```

Si bien 5 MB sigue siendo un poco mucho (y no incluye GTK ni nada, aunque no jugué con las opciones del compilador) para un simple binario, a partir de ahora FreePascal + Lazarus en mi ranking personal entran en la categoría de "interesantes" :-)

Es bastante tarde, es cierto, más cuando gracias a [Python](http://www.python.org) disfruto y me divierto al programar; tengo todo lo que necesito y más que lo que podría tener con FreePascal + Lazarus (siempre lo digo, Python no sería Python si no fuera por [PyAr](http://www.python.com.ar/)), pero... bueno, rescato y festejo que haya otra herramienta de desarrollo libre, potente y al alcance de cualquiera. :-)

Saludos
