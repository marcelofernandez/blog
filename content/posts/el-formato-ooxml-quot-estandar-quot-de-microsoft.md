---
author: mfernandez
category:
  - codear
  - misceláneos
date: "2007-01-04T03:08:00+00:00"
guid: https://blog.marcelofernandez.info/?p=40
title: El formato OOXML &quot;estándar&quot; (?) de Microsoft
url: /2007/01/el-formato-ooxml-estandar-de-microsoft/

---
Leyendo [este post de Rob Weir](http://www.robweir.com/blog/2006/01/how-to-hire-guillaume-portes.html) me indigno muchísimo (otra vez), con [esta empresa](http://www.microsoft.com/) que lo único que hace es mentir. Me pueden decir en qué [supuesto estándar de documentación](http://en.wikipedia.org/wiki/OOXML) (XML en este caso) pueden figurar estos tags? :  

- autoSpaceLikeWord95 (Emulate Word 95 Full-Width Character Spacing)
- footnoteLayoutLikeWW8 (Emulate Word 6.x/95/97 Footnote Placement)
- mwSmallCaps (Emulate Word 5.x for the Macintosh Small Caps Formatting)
- suppressTopSpacingWP (Emulate WordPerfect 5.x Line Spacing)
- **lineWrapLikeWord6** (Emulate Word 6.0 Line Wrapping for East Asian Text)
- **mwSmallCaps** (Emulate Word 5.x for Macintosh Small Caps Formatting)
- **shapeLayoutLikeWW8** (Emulate Word 97 Text Wrapping Around Floating Objects)
- **truncateFontHeightsLikeWP6** (Emulate WordPerfect 6.x Font Height Calculation)
- **useWord2002TableStyleRules** (Emulate Word 2002 Table Style Rules)
- **useWord97LineBreakRules** (Emulate Word 97 East Asian Line Breaking)
- **wpJustification** (Emulate WordPerfect 6.x Paragraph Justification)
- shapeLayoutLikeWW8 (Emulate Word 97 Text Wrapping Around Floating Objects)  

Como dice Rob Weir al final del post:  
"So not only must an interoperable OOXML implementation first acquire and reverse-engineer a 14-year old version of Microsoft Word, you must also do the same thing with a 16-year old version of WordPerfect. Good luck."

Traducción:  
"Entonces, una implementación interoperable con OOXML no sólo debe adquirir y hacer ingeniería reversa de una versión de Word de hace 14 años, también debe hacerse lo mismo con una versión de WordPerfect de hace 16 años. Buena suerte".

Esto no es un estándar, se trata de otro [chamuyo](http://es.wikipedia.org/wiki/Chamuyo#Chamuyar) más para hacerse ver ante los gobiernos (de que "su" formato de documentación es estándar. Por lo que veo, igual [la ECMA lo aprobó](http://www.ecma-international.org/news/PressReleases/PR_TC45_Dec2006.htm) (yo me pregunto, no sabrán nada como los que conceden [patentes a cosas como un algoritmo de lista enlazada](http://yro.slashdot.org/article.pl?sid=06/11/23/1546218&from=rss)?). Sí, lo sigo afirmando aunque en el comité técnico del ECMA hayan participado representativos de Apple, la Biblioteca Británica, Canon, Intel, Microsoft, Novell, Pioneer, Toshiba, y la Biblioteca del Congreso de los Estados Unidos. (Qué hace Novell ahí?? ummm....)

En definitiva, más mentiras y verdades a medias de la que ya me tiene acostumbrado.

Saludos  
Marcelo

Actualización: Sí, es indignante. Alguien [leyó esto????](http://www.techworld.com/opsys/news/index.cfm?newsid=7675)  
"The O/S will use much more of a PC's CPU resource because 'Vista's content protection requires that devices (hardware and software drivers) set so-called "tilt bits" if they detect anything unusual ... Vista polls video devices on each video frame displayed in order to check that all of the grenade pins (tilt bits) are still as they should be.'  

Also 'In order to prevent tampering with in-system communications, all communication flows have to be encrypted and/or authenticated. For example content sent to video devices has to be encrypted with AES-128.' Encryption/decryption is known to be CPU-intensive.  

Device drivers in Vista are required to poll their underlying hardware every 30ms - thirty times a second - to ensure that everything appears correct. "

Si esto es cierto (todavía me cuesta creerlo), es el colmo.  
