---
layout: post
title: Rails - Login
image: login.PNG
logo: login.PNG
description: Creacíon de un login con Rails, Devise y Bootstrap
escritor: Freivin Campbell
comments: true
---

<p class="intro"><span class="dropcap">R</span>ails es un framework muy versátil que nos permite crear sitios rapidamente, la utilización de gemas "plugins" por llamarle de esa forma nos ayuda en el desarrollo.</p>

# Antes de comenzar
-Ruby On Rails
-> [Windows](https://freivincampbell.github.io/blog/ruby-on-rails-en-windows/)
/ [Linux](https://freivincampbell.github.io/blog/ruby-on-rails-en-ubuntu-16-04/)
-Editor de texto -> [Instalar Atom](https://freivincampbell.github.io/blog/atom/)

### Documentación de las gemas a utilizar
-Devise -> [Documentacion oficial](https://github.com/plataformatec/devise)
-Bootstrap -> [Documentacion oficial](https://github.com/twbs/bootstrap-sass)
-Fontawesome -> [Documentacion oficial](https://github.com/bokmann/font-awesome-rails)

### Comencemos

> Creamos un nuevo proyecto con el siguiente comando:

```shell
rails new login-rails
```

> Entramos al proyecto con el comando:

```shell
cd login-rails
```

> Con el editor de texto abrimos el proyecto, nos dirigimos al archivo llamado **Gemfile** y añadimos nuevas gemas a la lista las cuales serian:

```ruby
gem 'bootstrap-sass'
gem 'devise'
gem "font-awesome-rails"
```

> La gema de **Bootstrap** será utilizada para la creación de las grillas *columnas*, la de **Devise** será utilizada para la autenticación y por último la gema **Fontawesome** para el uso de iconos en nuestro proyecto.
> Luego de editar y guardar nuestros cambios en la terminal ejecutaremos el siguiente comando:

```shell
bundle install
```

> Al terminal debemos configurar las gemas instaladas, comenzaremos con la de **Bootstrap**
> El archivo ubicado en los assets de nuestro proyecto con el nombre application.css lo renombramos a application.scss, debemos saber que de ahora en adelante cualquier archivo llamado desde nuestro archivo *.scss* debe llamarse con el **@import 'nombre archivo';** así que nuestro archivo deberá de quedar de esta manera:

```css
@import "bootstrap-sprockets";
@import "bootstrap";
@import "font-awesome";
```

> En la misma carpeta de los assets buscamos el archivo que se encuentra en la carpeta de javascript con un nombre similar: **application.js** el archivo nos deberá quedar de la siguiente forma:

````
//= require jquery
//= require jquery_ujs
//= require turbolinks
//= require_tree .
````

> Con ello ya debemos tener nuestro proyecto configurado con las gemas CSS.

### Configuración de la *gema devise*

> Con el siguiente comando corremos el generador de **Devise**

````
rails generate devise:install
````

> La consola nos despliega una serie de instrucciones, nos indica que debemos de buscar el archivo llamado **development.rb** y colocar la siguiente linea en el código:

```ruby
 config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

> Debemos modificar nuestro root_path  pero esto lo realizaremos más adelante, de la misma forma nos indica que debemos de añadir un **notice** y un **alert**  para las notificaciones de error o advertencia de nuestra gema pero de la misma forma lo añadiremos más adelante para trabajar con el **CSS**.
> A continuación añadiremos el **MODEL** con *Devise* en mi caso se llamará user, escribimos en nuestra terminal el siguiente comando:

````
rails generate devise  user
````
> Devise hará su magia y nos generará los archivos necesarios para la migración, para la creación de nuestra tabla de usuarios con autenticación, escribimos en nuestra terminal el siguiente comando:

````
rake db:migrate
````
### Configuración de las vistas

> Devise nos permite administrar las vistas generadas por la gema, con ello escribiremos en nuestra terminal el siguiente comando para hacer uso de las vistas:

````
rails g devise:views
````
> Nos generara una carpeta donde contendrá todo lo relacionado con las vistas de **Devise**

<img src="{{ '/assets/img/login-1.PNG' | prepend: site.baseurl }}" alt="">

> Con nuestros nuevos archivos a la vista ya podremos colocar nuestros **Notice / Alert** que **Devise** nos indicó antes. Buscaremos el archivo *new.html.erb* y lo modificaremos por:

```ruby
<% flash.each do |key, value| %>
<div class="alert alert-<%= key %>">
  <a href="#" data-dismiss="alert" ></a>
  <ul style="list-style-type: none">
    <li>
      <% if key == "alert"%>
      <div class="alert alert-danger alert-dismissible">
        <a href="#" class="close" data-dismiss="alert" aria-label="close">&times;</a>
        <strong> <%= value %></strong>
      </div>
      <% end %>
    </li>
  </ul>
</div>
<% end %>
<div class="col-md-6 col-md-offset-3 box-login">
  <span><i class="fa fa-key  fa-5x"></i></span>
  <br>
  <br>
  <%= form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
  <div class="field">
    <%= f.label "Correo", :id=>"text-label" %><br />
    <%= f.email_field :email, autofocus: true, :placeholder=> "ejemplo@gmail.com", :required=> :true %>
  </div>
  <div class="field">
    <%= f.label "Contraseña", :id=>"text-label" %><br />
    <%= f.password_field :password, autocomplete: "off", :placeholder=> "Contraseña", :required=> :true  %>
  </div>
<br>
<div class="actions">
  <%= f.submit "Iniciar Session", :class=> 'login' %>
</div>
<% end %>
<%= render "devise/shared/links" %>
</div>
```

> La vista está casi que lista, pero aun no le podemos ver así que en nuestro controlador principal colocaremos la siguiente línea de código:

```ruby
before_action :authenticate_user!
```

> Todos los controladores heredan de ApplicationController, así que cualquier callback pasara por aqui y se comprobará si el usuario tiene una sesión activa.

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
  before_action :authenticate_user!
end
```

> Esto comprueba, si se encuentra algún usuario logeado nos llevara a la ruta principal, la ruta aún no se encuentra asignada, crearemos un controlador de bienvenida el cual únicamente nos llevará a un html con un **Hello Word!** para saber que el login fue realizado con éxito.
> Escribimos en la terminal:

````
rake g controller Welcome index
````
> **Rails** nos escribe las rutas para el **controlador** creado así que únicamente indicaremos cual será el *root_path* para que **Devise** realice el redirect. Nos movemos hacia el archivo de rutas que se encuentra en la carpeta de **config** y deberá de quedar de la siguiente manera:

```ruby
Rails.application.routes.draw do
  resources :welcome, only: [:index]
  root 'welcome#index'

  devise_for :users
end
```

> De esta forma cada vez que se quiera acceder a rutas protegidas **Devise** hará su magia y nos llevará a la página de login donde podremos iniciar sesión, si ya poseemos una cuenta  o podremos crear una nueva cuenta.

<img src="{{ '/assets/img/login-2.PNG' | prepend: site.baseurl }}" alt="">

> La vista de crear usuarios aún no se encuentra completa así que en el archivo **new.html.erb** en la carpeta **registrations** modificamos de tal manera:

```ruby
<% flash.each do |key, value| %>
<div class="alert alert-<%= key %>">
  <a href="#" data-dismiss="alert" ></a>
  <ul style="list-style-type: none">
    <li>
      <% if key == "alert"%>
      <div class="alert alert-danger alert-dismissible">
        <a href="#" class="close" data-dismiss="alert" aria-label="close">&times;</a>
        <strong> <%= value %></strong>
      </div>
      <% end %>
    </li>
  </ul>
</div>
<% end %>
<div class="col-md-6 col-md-offset-3 box-login">
  <span><i class="fa fa-users  fa-5x"></i></span>
  <br>
  <br>
  <%= form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
  <div class="field">
    <%= f.label "Correo", :id=>"text-label" %><br />
    <%= f.email_field :email, autofocus: true, :placeholder=> "ejemplo@gmail.com", :required=> :true %>
  </div>
  <div class="field">
    <%= f.label "Contraseña", :id=>"text-label" %><br />
    <%= f.password_field :password, autocomplete: "off", :placeholder=> "Contraseña", :required=> :true  %>
  </div>
  <div class="field">
    <%= f.label "Repetir Contraseña", :id=>"text-label" %><br />
    <%= f.password_field :password_confirmation, autocomplete: "off", :placeholder=> "Contraseña", :required=> :true  %>
  </div>
<br>
<div class="actions">
  <%= f.submit "Crear Usuario", :class=> 'login' %>
</div>
<% end %>
<%= render "devise/shared/links" %>
</div>
```

> Perfecto si todo salio bien únicamente nos hace falta el CSS, recuerden todo archivo debe ser llamado con el **@import 'nombre_archivo';**  creamos un archivo llamado **login-css.scss** en assets y en la carpeta de **stylesheets**

1.login-css.scss

```css
body{
  font-family: "Lato", "Helvetica Neue", Helvetica, Arial, sans-serif;
    font-size: 15px;
    line-height: 1.42857143;
    color: #ebebeb;
    background-color: #2b3e50;
}
form {
   // border: 3px solid #f1f1f1;
}
input[type=text], input[type=password], input[type=email] {
 width: 75%;
 padding: 12px 20px;
 margin: 8px 0;
 display: inline-block;
 border: 1px solid #ccc;
 box-sizing: border-box;
}

.login {
 background-color: #4CAF50;
 color: white;
 padding: 14px 20px;
 margin: 8px 0;
 border: none;
 cursor: pointer;
 width: 75%;
}

.box-login {
 margin-top: 1%;
 text-align: center;
 // top: 50%;
}
#text-label{
 font-size: 1.35em;
 text-align: left;
}
```
> El archivo **application.scss** deberá de verse de la siguiente manera:

```css
@import "bootstrap-sprockets";
@import "bootstrap";
@import "font-awesome";
@import "login-css";
```

> Para terminar corremos en la terminal:

````
rails s
````
> Revisamos en nuestro navegador **localhost:3000** y debería de verse así:
> Vista Final


<img src="{{ '/assets/img/login.PNG' | prepend: site.baseurl }}" alt="">


#### Link del repositorio en Github para la Descarga


-[LINK](https://github.com/Freivincampbell/login-rails)


> Seria todo, con esto terminamos, si tienen alguna pregunta, duda o comentario no duden en escribirme acá abajo.
>
> Espero que hayas aprendido algo nuevo y útil. :smile:
