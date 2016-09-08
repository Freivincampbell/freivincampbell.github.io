---
layout: post
title: laravel en heroku
image: laravel.jpg
logo: laravel.jpg
description: Subir un proyecto de Laravel en Heroku con Postgresql
escritor: Freivin Campbell
comments: true
---

<p class="intro"><span class="dropcap">E</span>sta guía le guiará a través de la configuración y el despliegue de una aplicación laravel 5. Para una introducción general a la utilización de PHP en Heroku.</p>

## Requerimientos previos

1. PHP [instalación aquí](https://secure.php.net/)
2. Cuenta en Heroku [Crear una aquí](https://signup.heroku.com/dc)
3. Composer [instalación aquí](https://getcomposer.org/download/)
4. CLI de Heroku [instalación aquí](https://devcenter.heroku.com/articles/heroku-command-line)
5. Git [instalación aquí](https://git-scm.com/)

## Creación de una aplicación en laravel

> Podemos crearla de dos formas con composer y también con laravel.

1. Laravel.

````
laravel new laravel_en_heroku
````

2. Composer.

````
composer create-project --prefer-dist laravel/laravel laravel_en_heroku
````

> Con el proyecto ya creado entraremos a la carpeta y ejecutamos el comando para iniciar un nuevo repositorio.

```
git init
```

>Al ejecutar dicho comando creara un nuevo **repositorio** en nuestro proyecto cual será de utilidad para subirlo a **Heroku**, pero tendremos que hacer unos pequeños cambios en los archivos que nos creó el comando.

>En el archivo **.gitignore** borraremos la línea que dice **.env**.

>Continuando con la configuración crearemos un archivo en la raíz del proyecto con el nombre: **Procfile** y la primer línea escribiremos:

1. Copiando y pegando.

```
web: vendor/bin/heroku-php-apache2 public
```

2. Comando.

````
echo web: vendor/bin/heroku-php-apache2 public/ > Procfile
````

>En archivo le estamos indicando al proyecto en php que la ruta por defecto comenzara en public **_en mi caso_** ya que desde ahí es donde cargo el CSS y las imágenes.

>Crearemos nuestra app en **heroku** para ello debemos estar logueados ejecutamos el siguiente comando:

````
heroku create laravel_en_heroku
````

>Para que la app en **Heroku** sepa que el proyecto que subiremos es de **PHP** configuraremos la aplicación para esto:

````
heroku buildpacks:set heroku/php
````

>**Heroku** utiliza la base de datos por defecto de **Postgresql**, así que crearemos una base de datos para configurarla y utilizarla con nuestro proyecto.

````
heroku addons:add heroku-postgresql:hobby-dev
````

>Podemos ver la configuración de nuestra aplicación y de nuestra base de datos con el siguiente comando:

````
heroku config
````

>La que nos interesa en este momento es la de la **base de datos** que se vería algo así:

````
DATABASE_URL: postgres://xwqoettsiiewgo:pu63JXg0ZG2TI2FVE3FHH0g16J@(ec2-44-443-42-104.compute-1.amazonaws.com):5432/d3f0a48r3kq532
````

>Esa es la información que utilizaremos para **configurar** nuestra base de datos de **laravel** para **Heroku**, en el archivo **.env** buscaremos la configuración de la base de datos y la cambiaremos de la siguiente forma:

````
DB_CONNECTION= pgsql
DB_HOST= colocamos lo que esta después del @ antes de los :
DB_DATABASE= colocamos lo que esta después del / hasta el final
DB_USERNAME= después de los dos // y antes de los :
DB_PASSWORD= después de los : y antes de @
````

>Debería de verse así:

````
DB_CONNECTION= pgsql
DB_HOST= ec2-44-443-42-104.compute-1.amazonaws.com
DB_DATABASE= d3f0a48r3kq532
DB_USERNAME= xwqoettsiiewgo
DB_PASSWORD= pu63JXg0ZG2TI2FVE3FHH0g16J
````

>En el archivo **database** en la carpeta **config** aproximadamente en la línea 29 debería de quedar de esta manera:

````
.
├── config
    ├── database.php
````

````
'default' => env('DB_CONNECTION', 'pgsql'),
````

>La configuración e instalación está lista ya la podremos subir a **Heroku** con esto ejecutamos los siguientes comandos:

````
git add .
git commit -m "Deploying to Heroku"
git push heroku master
heroku run php artisan migrate
heroku open
````

> ¡Y eso es! Si ahora abre su navegador, ya sea de forma manual señalando a la URL de heroku creada, o mediante el uso de la CLI Heroku para ver la página con **heroku open**.

> Espero que hayas aprendido algo nuevo y útil. :smile:
