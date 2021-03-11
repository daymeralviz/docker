# docker
study docker

Comandos:
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