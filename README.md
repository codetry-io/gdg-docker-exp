<p align="center">
<img src="http://codetry.io/logo-mail.png">
<img width="120" src="http://gdgneuquen.org/wp-content/themes/gdgnqn/images/logo.png">
</p>


# Docker para Web Devs

Repositorio para almacenar ejemplos de uso de docker-compose de la charla _**Docker para Web Devs**_ (  [Juan Pablo Orlando](https://github.com/JuampiOrlando) y [Martin Moreira](https://github.com/morexlt) ) en el encuentro [#WebAFull2018](https://www.meetup.com/es-ES/gdgneuquen/events/247507837/), [GDG Neuquen](https://www.meetup.com/gdgneuquen/?_locale=es-ES). 

## Presentación

La presentación se encuentra en el raiz del repo como _Presentacion.pdf_

## Requisitos

Tener instalado [Docker](https://docs.docker.com/install/).

Tener instalado **docker-compose**. (En el caso de Linux, dependiendo de la distribucion, hay que instalar docker-compose por separado).

## Instrucciones

### Ejemplo_node

En el ejemplo de node tenemos 3 containers, uno de **mongodb**, otro para nuestra api en **node** y un **apache** para nuestro angular. Otra alternativa seria utilizar el container de node para servir el _index.html_ del proyecto de angular.
* Api
  * Ejecuta en el puerto 3030 (cambiarlo a 8080 o al que tengan configurado en node)
  * Existe una variable de entorno **mongodb**, por si quieren usarla; no es necesario.
  * El comando para iniciar es **npm install && npm start --production**. Pueden cambiarlo por el que necesiten.
  * En el item _volumes_: tenemos **- ./ejemplo_api/:/home/node/app** ,siendo _ejemplo_api_ la carpeta contenedora de la api.
* App
  * En el item volumes: tenemos **- ./ejemplo_app/dist/:/usr/local/apache2/htdocs** ,siendo _ejemplo_app_ la carpeta contenedora del angular compilado.

### Ejemplo_php

En el ejemplo de php tenemos 2 containers, uno de **mysql**, y una imagen armada (DockerFile) desde **ubuntu** en donde instalamos todo para que funcione hasta **Laravel**.
* Webserver
  * Este como veran en vez de especificar **imagen:** tiene **build: .** el cual le indica a docker que existe un Dockerfile en la misma carpeta que tiene que construir.
  * Hay 2 volumenes, uno monta el codigo que se encuentra en la misma carpeta que el archivo docker-compose.yml y el otro es el apache.conf que es una configuracion especifica para que funcione el router de Laravel.
* Database
  * La DB tiene seteadas las variables de entorno a fin de que construya el container con esos datos de usuario y db.

## Network (Importante!!)

Como se explicó en el meetup docker arma una sub-red con todos los containers que se encuentran en el archivo docker-compose.yml por lo tanto en los dos ejemplos cuando querramos acceder a la base de datos por ejemplo, no debemos utilizar localhost o 127.0.0.1 ya que ese no será el IP. Docker nos provee de un _DNS local_, por lo que podemos llamarlo por el nombre que le dimos al servicio en el archivo **docker-compose.yml**. Es decir, a la db de mongo, la podemos acceder como **mongodb://ejemplo_db:27017/ejemplo_api** mientras que el mysql estará en **db:3306**

## Ejecución

Para cualquier de los dos ejemplos: 
1. **docker-compose up** o **docker-compose up -d** para dejarlo como deamon corriendo.
2. **docker-compose stop** para detenerlos. 

**OBS**: Los comandos siempre deben ser ejecutados parados en la carpeta donde se encuentra el archivo **docker-compose.yml**
