---
layout: post
title: Mi primer API con Ruby on Rails
image: mi-primer-api/api.JPG
logo: mi-primer-api/api.JPG
description: API es un servicio que facilita la creación de servicios HTTP disponibles para una amplia variedad de clientes.
escritor: Freivin Campbell
comments: true
---

<p class="intro"><span class="dropcap">U</span>n API es un servicio que facilita la creación de servicios HTTP disponibles para una amplia variedad de clientes, entre los que se incluyen exploradores y dispositivos móviles. Rails es la plataforma perfecta para crear aplicaciones RESTful de una manera fácil, rápida y sin olvidar sencilla.</p>

# Comencemos
 > Con la llegada de **Rails 5** la vida para los amantes del API se volvió más sencilla, este maravilloso **framework** nos ofrece en su nueva versión una manera aún más sencilla para la creación de API´s, ahora con un solo comando tendremos la estructura para nuestro **BACK-END**.

> Para instalar ruby on rails en tu equipo aqui te dejo un tutorial para que puedan **instalarlo** esta escrito para la versión **4.2.6** el unico detalle seria cambiar la versión por la actual de rails, **adjunto** el link de rails para que vean la versión final:
[rubyonrails.org](https://goo.gl/QhOCmx)


## Instalar Rails en Ubuntu
[Ubuntu](https://goo.gl/y3Lp8G)

## Instalar Rails en Windows
[Windows](https://goo.gl/yjFmnp)

> Para los que ya tienen **ruby** instalado, instalar **Rails** en su versión **5** debemos correr el siguiente comando:

```shell  
gem install rails -v 5.0.2
```

> Al finalizar la instalación tenemos nuestro **framework** completamente preparado para el uso de **API's** así que corremos:

```shell  
rails -v
```

> y corroboramos que esté instalada la versión de rails correspondiente

> Perfecto **Rails** es tan *versátil* que con un solo comando creará la estructura correspondiente para un proyecto y además quedará listo para usarlo al 100% como un **API**. en la terminal nos colocamos donde crearemos el proyecto y escribimos:

```shell  
rails new [nombre del proyecto] --api
```

> Listo, con ello podremos abrir el proyecto con nuestro editor de texto favorito.
> Para crear un **CRUD** de libros **Rails** nos ayuda con el **scaffold** así que en nuestra terminal posicionados en la carpeta de nuestra API corremos el siguiente comando:


```shell  
rails generate scaffold book title author synopsis year
```

> **Debemos** recordar que cuando se generan migraciones, modelos y scaffold, si los atributos van en blanco el asume que son de tipo **string** así que para el ejemplo únicamente escribiremos: el título del libro, el autor, la sinopsis y por último el año del libro.

> Le damos enter y **Rails** hará su *MAGIA*, nos creará la migración para la base de datos, el modelos de los libros, el controlador, y los test para el mismo.

```shell  
Running via Spring preloader in process 26563
      invoke  active_record
      create    db/migrate/20170326180203_create_books.rb
      create    app/models/book.rb
      invoke    test_unit
      create      test/models/book_test.rb
      create      test/fixtures/books.yml
      invoke  resource_route
       route    resources :books
      invoke  scaffold_controller
      create    app/controllers/books_controller.rb
      invoke    test_unit
      create      test/controllers/books_controller_test.rb

```

> Antes de correr y generar nuestra tabla podemos revisar nuestra **migración** y asegurarnos que todo esté bien, revisamos el archivo que se encuentra en db/migrate/[fecha y nombre de la migración] nos indica lo siguiente

> Creación de una tabla llamada libros la cual contiene título, autor, sinopsis y año.

```ruby  
class CreateBooks < ActiveRecord::Migration[5.0]
  def change
    create_table :books do |t|
      t.string :title
      t.string :author
      t.string :synopsis
      t.string :year

      t.timestamps
    end
  end
end

```

> Con ello podremos correr la migración con el siguiente comando:

```shell  
rails db:migrate
```

> Por el momento estamos usando la base de datos por defecto **sqlite3** así que no hay que pensar mucho en eso.

```shell  
== 20170326180203 CreateBooks: migrating ======================================
-- create_table(:books)
   -> 0.0024s
== 20170326180203 CreateBooks: migrated (0.0025s) =============================
```

> Si entramos en nuestra consola de rails con el comando:

```shell  
rails console
```

> escribimos

```ruby  
Book.all
```
> nos deberá retornar un select * from books de la base de datos y en nulo ya que no creamos libros aun.

```sql  
SELECT "books".* FROM "books"
```

> Podemos echarle un vistazo a nuestro controlador de libros ubicado en app/controllers/books_controller.rb en él podemos encontrar los métodos **CRUD** ya escritos y funcionales en el.

```ruby  
class BooksController < ApplicationController
  before_action :set_book, only: [:show, :update, :destroy]

  # GET /books
  def index
    @books = Book.all

    render json: @books
  end

  # GET /books/1
  def show
    render json: @book
  end

  # POST /books
  def create
    @book = Book.new(book_params)

    if @book.save
      render json: @book, status: :created, location: @book
    else
      render json: @book.errors, status: :unprocessable_entity
    end
  end

  # PATCH/PUT /books/1
  def update
    if @book.update(book_params)
      render json: @book
    else
      render json: @book.errors, status: :unprocessable_entity
    end
  end

  # DELETE /books/1
  def destroy
    @book.destroy
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_book
      @book = Book.find(params[:id])
    end

    # Only allow a trusted parameter "white list" through.
    def book_params
      params.require(:book).permit(:title, :author, :synopsis, :year)
    end
end

```

> Probemos su funcionamiento; para mi persona recomiendo usar **postman** para probar las peticiones HTTP, lo pueden descargar de [aquí](https://goo.gl/AwfgR8). Es muy sencillo de utilizar.

> Recordemos encender el server de rails con un:

```shell  
rails s  
```

> Colocamos el tipo de petición en el caso de crear necesitamos un **POST**, escribimos la **url** a la cual mandaremos la petición y cambiamos la opción que dice **text** por la opción que dice **JSON (application/json)**

>Debería de verse algo así:

<img src="{{ '/assets/img/mi-primer-api/postman1.png' | prepend: site.baseurl }}" alt="">


> Escribimos el cuerpo del mensaje en formato json y luego de ello presionamos el botón send y postman deberá regresarnos un código 201 de creado y el objeto en un json

<img src="{{ '/assets/img/mi-primer-api/postman2.png' | prepend: site.baseurl }}" alt="">

> Si no conoce las url de tu proyecto en la terminal escribiendo el siguiente comando nos mostrara la estructura de cada una de las **rutas** de nuestro proyecto

```shell  
rails routes
```

```shell  
Prefix Verb   URI Pattern          Controller#Action
 books GET    /books(.:format)     books#index
       POST   /books(.:format)     books#create
  book GET    /books/:id(.:format) books#show
       PATCH  /books/:id(.:format) books#update
       PUT    /books/:id(.:format) books#update
       DELETE /books/:id(.:format) books#destroy

```

> Para saber cuántos libros posee la base de datos al cambiar post pot get realizará un select a la base de datos y nos retornara lo que encuentre :

<img src="{{ '/assets/img/mi-primer-api/postman3.png' | prepend: site.baseurl }}" alt="">

> El código de regreso ya no es un 201 sino un 200 cual es correspondiente si un **get** fue realizado con éxito.

### Despedida
> Seria todo, con esto terminamos, si tienen alguna pregunta, duda o comentario no duden en escribirme acá abajo. Nos vemos en el siguiente **POST**
>
> Espero que hayas aprendido algo nuevo y útil. :smile:
