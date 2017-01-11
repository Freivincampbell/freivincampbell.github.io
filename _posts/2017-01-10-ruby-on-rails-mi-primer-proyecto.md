---
layout: post
title: Mi primer proyecto con Rails
image: rails-primer-proyecto/rails.jpg
logo: rails-primer-proyecto/rails.jpg
description: Crear mi primer proyecto con Rails.
escritor: Freivin Campbell
comments: true
---

<p class="intro"><span class="dropcap">R</span>uby on Rails es un framework de aplicación web extremadamente productivo escrito en Ruby. Está diseñado para hacer que la programación de aplicaciones web sea más fácil, haciendo supuestos sobre lo que cada desarrollador necesita para comenzar.</p>

# Comencemos

> Será necesario tener instalado Ruby y Rails acá les dejo los link para que lo descarguen e instalen en [WINDOWS](https://goo.gl/yjFmnp) y [UBUNTU](https://goo.gl/y3Lp8G).
>
> Después de terminar la instalación crearemos una carpeta donde trabajar con los proyectos. En mi caso sería:

```shell  
mkdir rails
cd rails
```

> Para crear un nuevo proyecto con **Rails** debemos escribir en la **consola**:

```shell  
rails new demo
```
> De esta manera Rails hará lo suyo, nos crea una estructura base para trabajar.

-   Ejemplo:

<img src="{{ '/assets/img/rails-primer-proyecto/rails-1.PNG' | prepend: site.baseurl }}" alt="">

### Iniciando el servidor de Rails

> Para poder ver el servidor corriendo únicamente debemos escribir el siguiente comando en la terminal:

```shell  
rails server
```
> o bien:

```shell  
rails s
```

> Si todo sale bien al escribir en nuestro *navegador*: **[localhost:3000](http://localhost:3000/)** debemos ver la pantalla de bienvenida de **Rails**:

-   Ejemplo

<img src="{{ '/assets/img/rails-primer-proyecto/rails-2.PNG' | prepend: site.baseurl }}" alt="">  

> Seria todo, con esto terminamos, si tienen alguna pregunta, duda o comentario no duden en escribirme acá abajo.
>
> Espero que hayas aprendido algo nuevo y útil. :smile:
