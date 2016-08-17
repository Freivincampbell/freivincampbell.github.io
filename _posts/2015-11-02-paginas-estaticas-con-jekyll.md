---
layout: post
title: Páginas estáticas con Jekyll
description: esto es una prueba para ver si funciona
image: touring.jpg
author:
escritor: Freivin Campbell
subtitle: Un generador estático para blogs y websites en Ruby
category: Posts
comments: true
---

Hoy en dia muchos sitios web esta elaborados en una herramienta conocida como gestores de contenido o simplemente CMS(Content Managment System), tal vez hayas escuchado este termino o hayas usado uno, como Worpress, Joomla, Drupal, etc, pues bien, se puede decir  que Jekyll es  algo parecido, una herramienta para administrar contenido.

# ¿Que es Jekyll?

Como hemos dicho Jekyll es una herramienta para administrar contenido. Es un generador de sitios estáticos.

Para su funcionamiento no necesitamos usar una base de datos, incluso es unos de los objetivos de Jekyll,  no  depender de las  configuraciones y concentrarse  en lo que mas importa, el contenido. Lo que hace Jekyll es simplemente convertir  archivos de texto a Html.

# Construcción del sitio

### Como empezar a trabajar con Jekyll

Antes de comenzar a crear  nuestro sitio web o nuestro blog, debemos instalar jekyll, para poder trabajar con  Jekyll necesitamos tener instalado  Ruby, Instalamos Ruby de esta manera:

<div class="terminal">
sudo apt-get install ruby-full
</div>

Y con ello podremos instalar la gema de Jekyll con un simple comando

<div class="terminal">
gem install jekyll
</div>

Luego de instalar Jekyll empezamos en crear el primer sitio:

<div class="terminal">
jekyll new site
</div>

ó

<div class="terminal">
~/site/$ jekyll new .
</div>

si ya tienes la carpeta creada

Luego entramos a la carpeta o directorio que acabamos de crear con Jekyll y corremos el servidor:

<div class="terminal">
cd site
~/site/$ jekyll s
</div>

Puedes mirar el sitio creado, entrando a un navegador escribiendo la ruta  `http://localhost:4000/`

###Estructura de directorio

Jekyll por defecto  genera varias carpetas y archivos, en la documentación oficial puedes encontrar y  entender el objetivo de cada uno de ellos, véalo en el siguiente enlace <a href="http://jekyllrb.com/docs/structure/" target="\_blank">http://jekyllrb.com/docs/structure/</a>


  * **_posts:** Ahí se almacenan los artículos, los archivos deben tener un formato `'aaa-mm-dd-titulo'`, el nombre del archivo separado por guiones.

  * **_includes:** Se guardan los componentes que se reutilizan en todo el sitio web como los footers o los headers.

  * **_layouts:** Ahí se aguarda todas las plantillas, Jekyll por defecto crea 3 plantillas que son `default`, `page` y `post`, el `default` es la plantilla que va a usar todas las paginas, `page` es la plantilla que va a usar las paginas  que no son artículos y `post` es la plantilla que van usar nuestros artículos, `post` y `page` usan la plantilla `default`.

  * **_config.yml:** Archivo de configuración de Jekyll, ahi se especifican los datos generales del sitio Web.
  por ejemplo:
{% highlight html %}
  social:
  - title: twitter
    url: https://twitter.com/freivin12
  - title: github
    url: https://github.com/freivincampbell
  - title: google plus
    url: https://plus.google.com/u/0/+freivincampbell/posts
 {% endhighlight %}

### Markdown

Unos de los términos mas importantes que debemos aprender es el termino Markdown, ya que Jekyll soporta este formato  para escribir los posts. Markdown es una forma ligera para escribir textos para web, podemos escribir palabras itálicas, negrita, listas y mucho mas. Lo que hace markdown es convertir el texto plano a HTML, puedes encontrar muy buena información sobre markdown y como aplicarlo en el siguiente enlace <a href="https://guides.github.com/features/mastering-markdown/" target="\_blank">Mastering Markdown. </a>. Existen editores para poder trabajar con este Formato como son
<a href="http://dillinger.io/" target="\_blank">Dillinger</a> y <a href="http://pad.haroopress.com/" target="\_blank">Haroopad</a>. Para los archivos que soporten markdown se deben guardar con la extensión  md, markdown o textile.


### Liquid

Es el sistema de plantillas que usa Jekyll, desarrollado por <a href = "http://es.shopify.com/" target="\_blank">Shopify</a>, liquid  nos ofrece una serie de etiquetas, filtros y sentencias que nos ayuda a facilitar el desarrollo de nuestro sitio web, puedes encontrar mas información en la documentación oficial de <a href="https://github.com/Shopify/liquid/wiki" target="\_blank">Liquid</a>.

### Yaml Front Matter

En los templates y en los posts que por defecto  crea Jekyll, en la parte superior del archivo se encuentra un bloque de código con tres lineas en puntos, algo parecido como esto:

{% highlight html %}
---
layout: post
title: Blogging Like a Hacker
---
{% endhighlight %}

Ese bloque de código esta con un formato <a href="http://fdik.org/yml/" target="\_blank">YML</a>, que es un formato para la serialización de datos basado en el lenguaje XML. Jekyll usa <a href="http://jekyllrb.com/docs/frontmatter/" target="\_blank">
Yaml front Matter</a> para  definir  variables a los archivos, para luego  usarlas con las etiquetas de Liquid.  

# Sistema de comentarios

Para agregar un sistema de comentarios a nuestro sitio web podemos usar el servicio <a href="https://disqus.com/" >Disqus</a>. Para hacer uso de esta herramienta debes estar previamente registrado.

Para agregar el sistema de comentario debemos agregar la variable `comments` al YAML Front Matter con un valor de `true` al layout que queremos:

ejemplo:

{% highlight html %}
---
layout: default
comments: true
# other options
---
{% endhighlight %}

Luego añadimos el <a href="https://disqus.com/admin/universalcode/" target="\_blank">Código universal.</a>, en la plantilla donde quiera que cargue Disqus, debe ser colocado en el medio de las sentencia `% if page.comments %` y `% endif %`.

Para agregar un sistemas de comentarios en Jekyll, existen  diferentes maneras para hacerlo,  Disqus no es la única forma que existe. Para hacer un sistema de comentarios en Jekyll tambien se puede hacer con  <a href = "https://www.firebase.com/" target="\_blank">Firebase</a> o <a href="http://pooleapp.com/" target = "\_blank">pooleapp</a>, que son dos herramientas para servir datos dinámicos.


# Añadir formularios

Para añadir formularios a un sitio desarrollado por Jekyll es muy usual usar servicios externos, para los formularios los mas conocidos son:

 * **<a href="https://getsimpleform.com" target="_blank">Simple Form</a>**
 * **<a href="https://formkeep.com/" target="_blank">FormKeep</a>**  
 * **<a href="http://www.typeform.com/" target="_blank">TypeForm</a>**

Cada uno de estos servicios tiene una funcionalidad especial, que se  pueden usar para un mismo objetivo, si. Para poder utilizarlos solamente es coger el código de formulario Html que cada uno provee y pegarlo a donde quiera que  se vea formulario, estos servicios son muy buenos cuando deseen algún servicio de webhooks, un formulario de contacto o una encuesta.
