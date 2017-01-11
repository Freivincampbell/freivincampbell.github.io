---
layout: post
title: Plugin para slides con Rails
image: sliders.PNG
logo: sliders.PNG
description: Crear mi propio plugin para presentaciones con Rails.
escritor: Freivin Campbell
comments: true
---
<p class="intro"><span class="dropcap">E</span>n este post crearemos nuestro propio app para la creación de presentaciones olvidándonos del amado Power Point </p>

# ¿Power Point?

> Si hablamos de crear presentaciones, sin duda, Power Point es el primer programa que se nos viene a la cabeza. El programa de Microsoft se encuentra en un estado privilegiado. Tanto que en ocasiones se asimila el nombre del programa al propio documento. Sin embargo, hay más alternativas. Crear nuestro propio plugin no esta de mas.

## Comencemos

> Para comenzar el primer paso sería la creación de un proyecto nuevo en **Rails** en mi caso se llamará *sliders*, con el siguiente comando en la consola:

```shell
rails new nombre_proyecto
```

> Con nuestro editor de texto favorito abriremos el proyecto que acabamos de crear.
>
> Creamos un controlador para hacer  el render de la vista que usaremos para mostrar nuestra presentación.

```shell
rails g controller sliders index
```

> Ahora configuramos las *rutas* para no tener que escribir la **URL** cada vez que queremos mostrar el proyecto, el archivo de **rutas** debería de verse así:

```ruby
  Rails.application.routes.draw do
    root 'sliders#index'
    resources :sliders, only: [:index]
  end
```

> Si corremos el servidor deberíamos de ver reflejado el controlador creado con el html que **Rails** genera.

```shell
rails s
```

> Debería de mostar esto en [**localhost:3000**:](localhost:3000)

```html
<h1>Sliders#index</h1>
<p>Find me in app/views/sliders/index.html.erb</p>
```

> Modificaremos la vista, copiamos el siguiente fragmento de código y lo pegamos en la vista.

```html
<nav>
  <ul class="nav-list">
    <li class="nav-item one" id="0"></li>
    <li class="nav-item" id="1"></li>
    <li class="nav-item" id="2"></li>
    <li class="nav-item" id="3"></li>
  </ul>
</nav>
```

> Imagen:

<img src="{{ '/assets/img/sliders-1.PNG' | prepend: site.baseurl }}" alt="">

> deberíamos de ver únicamente 4 puntitos negro igual que la imagen, a continuación pegamos el siguiente fragmento de código después de la etiqueta **nav**

```html
<div class="wrapper">
  <div class="slide-wrapper">
    <div class="slide current one" id="0">
      <h1 class="slide-title">Slider #0</h1>
    </div>
    <div class="slide two shrink" id="1">
      <h1 class="slide-title">Slider #1</h1>
    </div>
    <div class="slide three shrink" id="2">
      <h1 class="slide-title">Slider #2</h1>
    </div>
    <div class="slide four shrink" id="3">
      <h1 class="slide-title">Slider #3</h1>
    </div>
  </div>
</div>
```

> Imagen:

<img src="{{ '/assets/img/sliders-2.PNG' | prepend: site.baseurl }}" alt="">

> Listo si refrescamos podremos ver los **”slides”**, falta el CSS y el JS, cuando nos encontramos trabajando con archivos **.CSS** y con archivos **.js** o **.coffee**, el se encarga de adjuntarlos y así ver sus estilos aplicados de igual forma todo lo que tenga que ver con **Javascript** no debemos hacer nada en nuestro **head**.
>
> Únicamente renombramos el archivo sliders.scss  por **sliders.css** en nuestra carpeta de **stylesheets** y de igual forma renombramos nuestro **sliders.coffee** y le llamaremos **sliders.js**, copiamos y pegamos el código correspondiente para cada archivo.


-   sliders.js

```javascript
$(document).ready(function() {
  $(".nav-item, .fa-arrow-right, .fa-arrow-left").mouseover(function() {
    $(this).addClass("increase-size");
    $(this).removeClass("decrease-size");
  });

  $(".nav-item, .fa-arrow-right, .fa-arrow-left").mouseout(function() {
    $(this).removeClass("increase-size");
    $(this).addClass("decrease-size");
  });

  $(".nav-item").click(function() {
    var currentNav = $(this),
      navLinkId = $(this).attr("id"),
      slides = $(".slide"),
      currentSlide = $(slides)[navLinkId],
      slideBg = $(currentSlide).css("background-color");

    $(currentSlide).removeClass("shrink").addClass("current unshrink");
    $(".slide").not(currentSlide).addClass("shrink").removeClass("current unshrink");
    $(currentNav).css("background-color", slideBg);
    $(".nav-item").not(currentNav).css("background-color", "white");
  });

  $(this).on("keydown", function(e) {
    var code = e.keyCode || e.which;
    var currentSlide = $(".slide.current");
    var currentSlideId = $(currentSlide).attr("id");
    var currentNav = $(".nav-item")[currentSlideId];
    var previousSlide = currentSlide.prev();
    var previousSlideId = $(previousSlide).attr("id");
    var previousSlideBg = $(previousSlide).css("background-color");
    var previousNav = $(".nav-item")[previousSlideId];
    var nextSlide = $(".current").next();
    var nextSlideId = $(nextSlide).attr("id");
    var nextSlideBg = $(nextSlide).css("background-color");
    var nextNav = $(".nav-item")[nextSlideId];

    if (code == 37) {

      if ($(previousSlide).hasClass("slide")) {
        $(currentSlide).removeClass("current unshrink").addClass("shrink");
        $(previousSlide).addClass("current unshrink").removeClass("shrink");
        $(previousNav).css("background-color", previousSlideBg);
        $(currentNav).css("background-color", "white");
      }
    }

    if (code == 39) {
      if ($(nextSlide).hasClass("slide")) {
        $(currentSlide).removeClass("current unshrink").addClass("shrink");
        $(nextSlide).addClass("current unshrink").removeClass("shrink");
        $(nextNav).css("background-color", nextSlideBg);
        $(currentNav).css("background-color", "white");
      }
    }
  });
});
```

-   sliders.css

```css
@import url(http://fonts.googleapis.com/css?family=Source+Sans+Pro:200,300,400,600,700,900,200italic,300italic,400italic,600italic,700italic,900italic);
html,
body {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  font-family: "Source Sans Pro";
}

.wrapper {
  height: 100%;
  position: relative;
}

.slide {
  width: 100%;
  height: 100%;
  position: absolute;
  display: none;
  overflow: hidden;
  text-align: center;
  margin: 0 auto;
}

.current {
  display: block
}

nav {
  position: absolute;
  width: 100%;
  height: 50px;
  background: #2C3E50;
  text-align: center;
  z-index: 99;
}

.nav-list {
  margin: 0 auto;
}

.nav-item {
  height: 13px;
  width: 13px;
  -webkit-border-radius: 100%;
  border-radius: 100%;
  background-color: #fff;
  display: inline-block;
  margin: 0 10px;
  margin-top: 25px;
  opacity: 0.9;
}

.nav-item#0 {
  background-color: #4183D7;
}

.nav-item:hover {
  cursor: pointer;
  #4183D7 opacity: 1;
}

.nav-colored {
  background-color: #fff;
  -webkit-transition: background-color 2s ease;
  -moz-transition: background-color 2s ease;
  -ms-transition: background-color 2s ease;
  -o-transition: background-color 2s ease;
  transition: background-color 2s ease;
}

.one,
.nav-item.one {
  background: #4183D7;
}

.two {
  background: #E74C3C;
}

.three {
  background: #26A65B;
}

.four {
  background: #EB9532;
}

.slide-title {
  margin: 0 auto;
  top: 50%;
  position: relative;
  display: block;
  color: #fff;
  font-size: 2em;
  font-weight: 500;
}

p {
  margin: 0 auto;
  top: 51%;
  position: relative;
  display: block;
  color: #fff;
  font-size: 1.2em;
  font-weight: 500;
}

.increase-size {
  -webkit-transform: scale3d(1.5, 1.5, 1.5);
  -moz-transform: scale3d(1.5, 1.5, 1.5);
  -o-transform: scale3d(1.5, 1.5, 1.5);
  transform: scale3d(1.5, 1.5, 1.5);
  -webkit-transition: transform 0.5s ease;
  -moz-transition: transform 0.5s ease;
  -o-transition: transform 0.5s ease;
  transition: transform 0.5s ease;
}

.decrease-size {
  -webkit-transform: scale3d(1, 1, 1);
  -moz-transform: scale3d(1, 1, 1);
  -o-transform: scale3d(1, 1, 1);
  transform: scale3d(1, 1, 1);
  -webkit-transition: transform 0.2s ease;
  -moz-transition: transform 0.2s ease;
  -o-transition: transform 0.2s ease;
  transition: transform 0.2s ease;
}

@keyframes shrink {
  from {
    -webkit-transform: rotateY(0deg);
    -moz-transform: rotateY(0deg);
    -o-transform: rotateY(0deg);
    -ms-transform: rotateY(0deg);
    transform: rotateY(0deg);
  }
  to {
    -webkit-transform: rotateY(88deg);
    -moz-transform: rotateY(88deg);
    -o-transform: rotateY(88deg);
    -ms-transform: rotateY(88deg);
    transform: rotateY(88deg);
  }
}

@-webkit-keyframes shrink {
  from {
    -webkit-transform: rotateY(0deg);
    -moz-transform: rotateY0deg);
    -o-transform: rotateY(0deg);
    -ms-transform: rotateY(0deg);
    transform: rotateY(0deg);
  }
  to {
    -webkit-transform: rotateY(88deg);
    -moz-transform: rotateY(88deg);
    -o-transform: rotateY(88deg);
    -ms-transform: rotateY(88deg);
    transform: rotateY(88deg);
  }
}

@-moz-keyframes shrink {
  from {
    -webkit-transform: rotateY(0deg);
    -moz-transform: rotateY(0deg);
    -o-transform: rotateY(0deg);
    -ms-transform: rotateY(0deg);
    transform: rotateY(0deg);
  }
  to {
    -webkit-transform: rotateY(88deg);
    -moz-transform: rotateY(88deg);
    -o-transform: rotateY(88deg);
    -ms-transform: rotateY(88deg);
    transform: rotateY(88deg);
  }
}

.shrink {
  -webkit-animation: shrink 1s 1 ease forwards;
  -moz-animation: shrink 1s 1 ease forwards;
  -o-animation: shrink 1s 1 ease forwards;
  animation: shrink 1s 1 ease forwards;
  overflow: visible !important;
}

@keyframes unshrink {
  from {
    -webkit-transform: rotateY(88deg);
    -moz-transform: rotateY(88deg);
    -o-transform: rotateY(88deg);
    -ms-transform: rotateY(88deg);
    transform: rotateY(88deg);
  }
  to {
    -webkit-transform: rotateY(0deg);
    -moz-transform: rotateY(0deg);
    -o-transform: rotateY(0deg);
    -ms-transform: rotateY(0deg);
    transform: rotateY(0deg);
  }
}

@-webkit-keyframes unshrink {
  from {
    -webkit-transform: rotateY(88deg);
    -moz-transform: rotateY(88deg);
    -o-transform: rotateY(88deg);
    -ms-transform: rotateY(88deg);
    transform: rotateY(88deg);
  }
  to {
    -webkit-transform: rotateY(0deg);
    -moz-transform: rotateY(0deg);
    -o-transform: rotateY(0deg);
    -ms-transform: rotateY(0deg);
    transform: rotateY(0deg);
  }
}

@-moz-keyframes unshrink {
  from {
    -webkit-transform: rotateY(88deg);
    -moz-transform: rotateY(88deg);
    -o-transform: rotateY(88deg);
    -ms-transform: rotateY(88deg);
    transform: rotateY(88deg);
  }
  to {
    -webkit-transform: rotateY(0deg);
    -moz-transform: rotateY(0deg);
    -o-transform: rotateY(0deg);
    -ms-transform: rotateY(0deg);
    transform: rotateY(0deg);
  }
}

.unshrink {
  -webkit-animation: unshrink 3s 1 ease forwards;
  -moz-animation: unshrink 3s 1 ease forwards;
  -o-animation: unshrink 3s 1 ease forwards;
  animation: unshrink 3s 1 ease forwards;
  overflow: visible !important;
}

.fa {
  position: absolute;
  color: #fff;
  top: 50%;
  font-size: 4em;
  z-index: 99;
}

.fa-keyboard-o {
  top: 70%;
  left: 0;
  right: 0;
}

.ua {
  position: absolute;
  right: 20px;
  bottom: 20px;
  color: #fff;
  font-size: 2em;
}

.ua:hover .fa {
   color: #17BEBB;
   -webkit-transform: scale(1.5);
   -moz-transform: scale(1.5);
   -ms-transform: scale(1.5);
   -o-transform: scale(1.5);
   transform: scale(1.5);
   -webkit-transition: all 1s ease;
   -moz-transition: all 1s ease;
   -ms-transition: all 1s ease;
   -o-transition: all 1s ease;
   transition: all 1s ease;
}
```

#### Link del repositorio en Github para la Descarga

-   [LINK](https://goo.gl/wQ0agA)

> Seria todo, con esto terminamos, si tienen alguna pregunta, duda o comentario no duden en escribirme acá abajo.
>
> Espero que hayas aprendido algo nuevo y útil. :smile:
