---
layout: post
title: Fácil Fullscreen Menu
image: Fullscreen-menu/menu.PNG
logo: Fullscreen-menu/menu.PNG
description: Creación de un menu full screen con efecto tipo Overlay de una forma fácil.
escritor: Freivin Campbell
comments: true
---
<p class="intro"><span class="dropcap">E</span>n este post crearemos nuestro menu full screen, con la ayuda de CSS, Javascript.</p>

## Comencemos

> El **post** se trata de un proyecto con **CSS**, **JS** y **HTML** así que no hace falta tener algún proyecto de **Rails** para probarle.

> Crearemos una estructura base para un proyecto *WEB* un *archivo* **index.html**, una carpeta para el **CSS** y una para el **JS**, la estructura del proyecto debería de verse así;

````
fullscreen-menu
├── css
    ├── style.css
├── js
    ├── javascript.js
├── index.html
````

> La estructura de nuestro **HTML** debe de verse de esta manera

```html
<!DOCTYPE html>
<html>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Menu FullScreen</title>
<link rel="stylesheet" href="css/style.css">
<link href='http://fonts.googleapis.com/css?family=Open+Sans:300,400,700,800' rel='stylesheet' type='text/css'>
<link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
<body>


  <script src="http://code.jquery.com/jquery-2.2.4.min.js"></script>
  <script src="js/javascript.js" type="text/javascript"></script>
</body>
</html>
```

> Agregamos:

- link para el **CSS**
- Fuentes con **Google** desde un **CDN**
- Iconos con font awesome desde un **CDN**
- Jquery desde un **CDN**
- link para el **JS**

> Para continuar agregamos dentro de nuestras etiquetas **Body** el siguiente fragmento de código:

```html
<nav class="menu-overlay">
    <button class="menu-toggle menu-cerrar">
        <i class="fa fa-times" aria-hidden="true"></i>
    </button>

    <ul>
        <li><a href="#">Inicio</a></li>
        <li><a href="#">Nosotros</a></li>
        <li><a href="#">Servicios</a></li>
        <li><a href="#">Contacto</a></li>
    </ul>
</nav>


<button class="menu-toggle">
    <i class="fa fa-bars" aria-hidden="true"></i>
</button>
```

> Falta el **CSS** y darle funcionalidad con **javascript** en nuestros archivos correspondientes colocaremos el siguiente fragmento de código:

- javascript.js

```js
$('.menu-toggle').on('click', function(){
    $('.menu-overlay').toggleClass('full-menu');
})
```

- style.css

```css
.menu-overlay {
	visibility: hidden;
	display: -webkit-box;
	display: -ms-flexbox;
	display: flex;
		-webkit-box-pack: center;
		-ms-flex-pack: center;
	justify-content: center;
		-webkit-box-align: center;
		-ms-flex-align: center;
	align-items: center;
	position: fixed;
	top: 0;
	left: 0;
	height: 100%;
	width: 100%;
	opacity: 0;
	background-color: rgba(8, 8, 8, 0.8);
		-webkit-transform: scale(0.85);
	transform: scale(0.85);
		-webkit-transition: all 200ms ease-in-out;
	transition: all 200ms ease-in-out;
	z-index: 2;
}

.menu-overlay ul {
	list-style-type: none;
	padding: 0;
}

.menu-overlay ul li {
	margin: 0 0 30px 0;
}

.menu-overlay ul li a {
font-size: 50px;
	color: #fff;
	line-height: 50px;
}

.menu-overlay ul li a:hover,
.menu-overlay ul li a:focus {
	color: tomato;
}

.menu-overlay.full-menu {
	visibility: visible;
	opacity: 1;
		-webkit-transform: scale(1);
	transform: scale(1);
}

.menu-toggle {
	border: 0; color: #fff;
	background-color: transparent;
	font-size: 2em;
	cursor: pointer;
}

.menu-cerrar {
	position: absolute;
	top: 30px;
	right: 30px;
}
```

> **¡LISTO!** si abrimos el archivo *index.html* deberá de desplegar nuestro menú.

- Imagen:

<img src="{{ '/assets/img/Fullscreen-menu/menu-1.PNG' | prepend: site.baseurl }}" alt="">

>

#### Link del repositorio en Github para la Descarga

- [LINK](https://goo.gl/SavjC4)

> Seria todo, con esto terminamos, si tienen alguna pregunta, duda o comentario no duden en escribirme acá abajo.
>
> Espero que hayas aprendido algo nuevo y útil. :smile:
