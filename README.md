<p align="center">
<img src="http://codetry.io/logo-mail.png">
<img width="120" src="http://gdgneuquen.org/wp-content/themes/gdgnqn/images/logo.png">
</p>


# Docker para Web Devs

Repositorio para almacenar ejemplos de uso de docker-compose para el gdg neuquen

## Presentación

La presentación se encuentra en el root del repo como .pdf

## Requisitos

Tener instalado Docker.
docker-compose. (En el caso de Linux, depende la version quizas no venga con docker-compose instalado).

## Instrucciones

### Ejemplo_node

En el ejemplo de node tenemos 3 containers, uno de mongodb, otro para nuestra api en node y un apache para nuestro angular. Este ultimo no es indispensable pero por una configuracion especifica nosotros lo utilizamos. La alternativa seria servir con la misma api al index.html del proyecto de angular.
* Api
  * Ejecuta en el puerto 3030 (cambiarlo a 8080 o al que tengan configurado en node)
  * Existe una variable de entorno **mongodb**, por si quieren usarla, no es necesario.
  * El comando para iniciar es **npm install && npm start --production** cambiarlo por el que necesiten.
  * En el item volumes: esta como **- ./ejemplo_api/:/home/node/app** siendo ejemplo_api la carpeta contenedora de la api.
* App
  * En el item volumes: esta como **- ./turn-app/dist/:/usr/local/apache2/htdocs** siendo ejemplo_app la carpeta contenedora del angular, el cual se tiene que encontrar compilado, por eso busca en dist!

### Ejemplo_php

En el ejemplo de php tenemos 2 containers, uno de mysql, y una imagen armada desde ubuntu en donde instalamos todo para que funcione hasta Laravel.
* Webserver
  * Este como veran en vez de especificar **imagen:** tiene **build: .** el cual le indica a docker que existe un Dockerfile en la misma carpeta que tiene que construir.
  * Hay 2 volumenes, uno monta el codigo que se encuentra en la misma carpeta que el archivo docker-compose.yml y el otro es el apache.conf que es una configuracion especifica para que funcione el router de Laravel.
* Database
  * La DB tiene seteadas las variables de entorno a fin de que construya el container con esos datos de usuario y db.

## Network (Importante!!)

Como se explicó en el meetup docker arma una sub-red con todos los containers que se encuentran en el archivo docker-compose.yml por lo tanto en los dos ejemplos cuando querramos acceder a la base de datos por ejemplo, no debemos utilizar localhost o 127.0.0.1 ya que ese no será el IP. Docker nos provee de un DNS local, de esta forma podemos llamarlo por el nombre que le dimos al servicio en el archivo docker-compose.yml. Es decir, a la db de mongo, la podemos acceder como **mongodb://ejemplo_db:27017/ejemplo_api** mientras que el mysql estará en **db:3306**

## Ejecución

Para cualquier de los dos ejemplos, luego ejecutar **docker-compose up** o **docker-compose up -d** para dejarlo como deamon corriendo.
Para detener los contenedores **docker-compose stop** siempre parados en la carpeta donde se encuentra el archivo docker-compose.yml
