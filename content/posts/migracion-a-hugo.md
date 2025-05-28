---
title: "Diario de Migración a un SSG"
date: 2025-05-28
draft: false
tags: ["hugo", "wordpress", "generadores de sitios estáticos", "cloudflare", "github", "ssg"]
categories: ["desarrollo web"]
author: "Marcelo Fernández"
showToc: false
TocOpen: false
---

Después de casi 20 años (¡fa! - arrancó [acá](https://marcelosoft.blogspot.com/2006/08/presentacin.html)) de este ~~rejunte de pavadas~~ blog personal usando [WordPress](https://wordpress.org/), más una Base de Datos MySQL, pagando un hosting y demás costos asociados, enfrenté la decisión de:
  - O bien matarlo,
  - O bien mudarlo a algo más económico que cueste muchísimo menos trabajo mantenerlo.
  
Respecto a este último punto, siempre los [Static Site Generators (SSG)](https://en.wikipedia.org/wiki/Static_site_generator) me llamaron la atención. No sólo por eliminar los intermediarios componentes de software que creo innecesarios entre mis dedos y un lector, sino porque realmente **escribir rápido un par de párrafos en Markdown en un archivo de texto, comitear y pushear** es mucho, mucho más rápido que:
  - Loguearse en un website
  - _Bypassear_ el captcha
  - Escribir en una ventana chiquita 
  - Jugar con una toolbar para aplicar estilos,
  - Insertar imágenes a la vieja usanza como si usara un procesador de texto,
  - "Guardar" el draft, ver si queda _aesthetic_ visualmente, etc.
  - Todo esto conectado a Internet, claro.

Honestamente creo que escribir cosas en [Markdown](https://es.wikipedia.org/wiki/Markdown), para lo que normalmente quiero _dumpear_ de mi cerebro, es mucho más que suficiente. Lo cual me va a permitir darle un poco más de vida a este páramo de sitio en el rinconcito intergaláctico de la web.

Analizando alternativas, finalmente me decidí usar [Hugo](https://gohugo.io/), almacenando todo el contenido [en un repo de GitHub](https://github.com/marcelofernandez/blog) y con [Cloudflare Pages](https://www.cloudflare.com/developer-platform/products/pages/) haciendo de Frontend HTTPS. Básicamente, escribo un archivo de texto con un mínimo de estilo, comiteo, pusheo al repo y a los dos minutos ya está publicado.
 
En resumen, hace rato que venía dilatando tomar una decisión porque quería preservar en la mayor medida posible los posts anteriores, pero apoyándome en [Github Copilot](https://github.com/features/copilot) y un [plugin](https://github.com/ashishb/wp2hugo) para Hugo pude hacer la migra en un par de horas, para al menos tener lo básico.

## Sobre la Integración con GitHub y Cloudflare Pages

### 1. Repo en GitHub

Tener mi sitio en GitHub me da:
- Control de versiones completo
- La chance de que otros contribuyan
- Un flujo de trabajo con ramas para probar cambios

### 2. Despliegue con Cloudflare Pages

Cloudflare Pages me ofrece:
- Despliegues automáticos desde GitHub
- Red CDN global para que todo vuele
- TLS gratis y configuración mínima

## Lo que aprendí en el camino

### Dificultades comunes

- Para los comentarios hay que usar soluciones externas como [Disqus](https://disqus.com/) o [Utterances](https://utteranc.es/) (los que tenía los perdí, muchos no tenía y tampoco me importa demasiado)
- Algunos formatos especiales y shortcodes hay que arreglarlos a mano (WIP)
- Hay que configurar redirecciones para no perder SEO (WIP)

### Beneficios colaterales

Después de la migración:
- Los tiempos de carga bajaron más del 70%
- El sitio sacó puntaje perfecto en herramientas como Lighthouse/[PageSpeed Insights](https://pagespeed.web.dev/)
- Publicar contenido es tan simple como hacer un commit en Git

## Para cerrar

Migrar de WordPress a Hugo fue un laburo que requirió muy poca planificación, pero los beneficios en términos de rendimiento, seguridad y flujo de trabajo valieron totalmente la pena. Para blogs personales y sitios de contenido, esta arquitectura moderna ofrece un equilibrio ideal entre facilidad de uso y potencia técnica.


Saludos!

Marcelo


## Referencias
- [Documentación oficial de Hugo](https://gohugo.io/documentation/)
- [Cloudflare Pages](https://pages.cloudflare.com/)
- [Wordpress to Hugo Static site migrator](https://github.com/ashishb/wp2hugo)
