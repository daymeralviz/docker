# docker
study docker

Comandos: generales 
docker info
docker --version
$ docker run hello-world (corro el contenedor hello-world)
$ docker ps (muestra los contenedores activos)
$ docker ps -a (muestra todos los contenedores)
$ docker inspect <containe ID> (muestra el detalle completo de un contenedor)
$ docker inspect <name> (igual que el anterior pero invocado con el nombre)
$ docker run –-name hello-platzi hello-world (le asigno un nombre custom “hello-platzi”)
$ docker rename hello-platzi hola-platzy (cambio el nombre de hello-platzi a hola-platzi)
$ docker rm <ID o nombre> (borro un contenedor)
$ docker container prune (borro todos lo contenedores que esten parados)


Comandos: linux
$ docker run ubuntu (corre un ubuntu pero lo deja apagado)
$ docker ps -a (lista todos los contenedores)
$ docker -it ubuntu (lo corre y entro al shell de ubuntu)
-i: interactivo
-t: abre la consola

<h1>cat /etc/lsb-release (veo la versión de Linux)</h1>

estados de contenedores

Comandos: estados y ciclos de vida

$ docker ps -a (veo todos los contenedores)
$ docker --name <nombre> -d ubuntu -f <comando>
$ docker --name alwaysup -d ubuntu tail -f /dev/null (mantiene el contenedor activo)
$ docker exec -it alwaysup bash (entro al contenedor)
$ docker inspect --format ‘{{.State.Pid}}’ alwaysup (veo el main process del ubuntu)
desde Linux si ejecuto kill -9 <PID> mata el proceso dentro del contenedor de ubuntu pero desde MAC no funciona


Comandos: exponiendo 

$ docker run -d --name proxy nginx (corro un nginx)
$ docker stop proxy (apaga el contenedor)
$ docker rm proxy (borro el contenedor)
$ docker rm -f <contenedor> (lo para y lo borra)
$ docker run -d --name proxy -p 8080:80 nginx (corro un nginx y expongo el puerto 80 del contenedor en el puerto 8080 de mi máquina)
localhost:8080 (desde mi navegador compruebo que funcione)
$ docker logs proxy (veo los logs)
$ docker logs -f proxy (hago un follow del log)
$ docker logs --tail 10 -f proxy (veo y sigo solo las 10 últimas entradas del log)

Comandos: bind mounts -- vincular archivo o carpeta a bd u otro elemento

$ mkdir dockerdata (creo un directorio en mi máquina)
$ docker run -d --name db mongo
$ docker ps (veo los contenedores activos)
$ docker exec -it db bash (entro al bash del contenedor)
$ mongo (me conecto a la BBDD)

shows dbs (listo las BBDD)
use platzi ( creo la BBDD platzi)
db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
db.users.find() (veo el dato que cargué)
$ docker run -d --name db -v <path de mi maquina>:<path dentro del contenedor(/data/db mongo)> (corro un contenedor de mongo y creo un bind mount)

Comandos: Volumenes - temas de seguridad

$ docker volume ls (listo los volumes)
$ docker volume create dbdata (creo un volume)
$ docker run -d --name db --mount src=dbdata,dst=/data/db mongo (corro la BBDD y monto el volume)
$ docker inspect db (veo la información detallada del contenedor)
$ mongo (me conecto a la BBDD)

shows dbs (listo las BBDD)
use platzi ( creo la BBDD platzi)
db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
db.users.find() (veo el dato que cargué)

Comandos: insertar o extraer archivos de un contenedor 

$ touch prueba.txt (creo un archivo en mi máquina)
$ docker run -d --name copytest ubuntu tail -f /dev/null (corron un ubuntu y le agrego el tail para que quede activo)
$ docker exec -it copytest bash (entro al contenedor)
$ mkdir testing (creo un directorio en el contenedor)
$ docker cp prueba.txt copytest:/testing/test.txt (copio el archivo dentro del contenedor)
$ docker cp copytest:/testing localtesting (copio el directorio de un contenedor a mi máquina)
con “docker cp” no hace falta que el contenedor esté corriendo

Conceptos fundamentales de docker:imagenes

Comandos:

$ docker image ls (veo las imágenes que tengo localmente)
$ docker pull ubuntu:20.04 (bajo la imagen de ubuntu con una versión específica)

Comandos: crear imagenes

$ mkdir imagenes (creo un directorio en mi máquina)
$ cd imagenes (entro al directorio)
$ touch Dockerfile (creo un Dockerfile)
$ code . (abro code en el direcotrio en el que estoy)

##Contenido del Dockerfile##
FROM ubuntu:latest
RUN touch /usr/src/hola-platzi.txt (comando a ejecutar en tiempo de build)
##fin##

$ docker build -t ubuntu:platzi . (creo una imagen con el contexto de build <directorio>)
$ docker run -it ubuntu:platzi (corro el contenedor con la nueva imagen)
$ docker login (me logueo en docker hub)
$ docker tag ubuntu:platzi miusuario/ubuntu:platzi (cambio el tag para poder subirla a mi docker hub)
$ docker push miusuario/ubuntu:platzi (publico la imagen a mi docker hub)


Comandos: sistema de capas

$ docker history ubuntu:platzi (veo la info de como se construyó cada capa)
$ dive ubuntu:platzi (veo la info de la imagen con el programa dive)


Comandos: usando docker para desarrollar aplicaciones

$ git clone https://github.com/platzi/docker
$ docker build platziapp . (creo la imagen local)
$ docker image ls (listo las imagenes locales)
$ docker run --rm -p 3000:3000 platziapp (creo el contenedor y cuando se detenga se borra, lo publica el puerto 3000)
$ docker ps (veo los contenedores activos)


Comandos: Aprovechando el caché de capas para estructurar correctamente tus imágenes

$ docker build platziapp . (creo la imagen local)
$ docker run --rm -p 3000:3000 -v pathlocal/index.js:pathcontenedor/index.js platziapp (corro un contenedor y monto el archivo index.js para que se actualice dinámicamente con nodemon que está declarado en mi Dockerfile)

Comandos: Docker networking: colaboración entre contenedores

$ docker network ls (listo las redes)
$ docker network create --atachable plazinet (creo la red)
$ docker inspect plazinet (veo toda la definición de la red creada)
$ docker run -d --name db mongo (creo el contenedor de la BBDD)
$ docker network connect plazinet db (conecto el contenedor “db” a la red “platzinet”)
$ docker run -d -name app -p 3000:3000 --env MONGO_URL=mondodb://db:27017/test platzi (corro el contenedor “app” y le paso una variable)
$ docker network connect plazinet app (conecto el contenedor “app” a la red “plazinet”)

Docker Compose: la herramienta todo en uno

Comandos:
$ docker-compose up -d (crea todo lo declarado en el archivo docker-compose.yml)

Comandos: Subcomandos de Docker Compose

$ docker network ls (listo las redes)
$ docker network inspect docker_default (veo la definición de la red)
$ docker-compose logs (veo todos los logs)
$ docker-compose logs app (solo veo el log de “app”)
$ docker-compose logs -f app (hago un follow del log de app)
$ docker-compose exec app bash (entro al shell del contenedor app)
$ docker-compose ps (veo los contenedores generados por docker compose)
$ docker-compose down (borro todo lo generado por docker compose)


Comandos: Docker Compose como herramienta de desarrollo

$ docker-compose build (crea las imágenes)
$ docker-compose up -d (crea los servicios/contenedores) 
fig===docker-compose   alias  up -d
$ docker-compose logs app (veo los logs de “app”)
$ docker-compose logs -f app (hago un follow de los logs de “app”)

version: "3.8"

services:
  app:
    build: .
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    depends_on:
      - db
    ports:
      - "3000:3000"
    volumes:
      -  .:/usr/src/
      - /usr/src/node_modules
    command: npx nodemon index.js 

  db:
    image: mongo



Comandos: Compose en equipo: override

$ touch docker-compose.override.yml (creo el archivo override)
$ docker-compose up -d (crea los servicios/contenedores)
$ docker-compose exec app bash (entro al bash del contenedor app)
$ docker-compose ps (veo los contenedores del compose)
$ docker-compose up -d --scale app=2 (escalo dos instancias de app, previamente tengo que definir un rango de puertos en el archivo compose)
$ docker-compose down (borro todo lo creado con compose)