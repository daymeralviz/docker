https://platzi.com/clases/1434-docker-swarm/15880-que-es-swarm/


Guido Vilariño

1. El problema de la escala: qué pasa cuando una computadora sóla no alcanza

Ejecutar aplicaciones productivas: la aplicación debe estar lista para servir a las usuarios a pesar de situaciones catastróficas, o de alta demanda (carga).
Escalabilidad: Poder aumentar la potencia de cómputo para poder servir a más usuarios, o a peticiones pesadas.
Escalabilidad vertical: Más hardware, hay límite físico.
Escalabilidad horizontal: Distribuir carga entre muchas computadoras. es el más usado.
Disponibilidad: Es la capacidad de una aplicación o servicio de estar siempre disponible para los usuarios. prevé problemas con servidores, etc.
La escalabilidad horizontal y la disponibilidad van de la mano.

2. Arquitectura de Docker Swarm

La arquitectura de docker swarm tiene el esquema de servidores manager y workers.
Los manager son los servidores que administran la comunicación y recursos entre los contenedores.
Los workers son los nodos donde se ejecutan los contenedores.
Todos estos nodos deben tener instalado docker daemon, de ser posible la misma versión y visibles entre sí.

3. Preparando tus aplicaciones para Docker Swarm: los 12 factores
12 factores de la aplicación.
https://12factor.net/

Codebase, tu código debe estar en un repositorio y este debería estar en relación de 1 a 1 entre código y repositorio.
Depencias, deberían venir empaquetadas con la aplicación.
La configuración, debe ser parte de tu aplicación.
Backing Service, como bases de datos, deben ser tratados como servicios externos a la aplicación.
Build, Release, Run. estas tres fases deben estar separadas en tu aplicación.
Proccess, la ejecución de tu aplicación no puede depender de que exista cierto estado, todo proceso lo debe realizar de forma atómica, stayless.
Port binding, la aplicación debe poder exponerse a si misma, sin intermediarios.
Concurrencia, que la aplicación pueda correr con múltiples instancia en paralelo.
Disposabiliti, la aplicación debe estar diseñada para ser fácilmente destruible e iniciar rápidamente.
Dev/prod parity, lograr que entorno de desarrollo, sea los más parecido a producción.
Logs, Todos los logs de la aplicación deben tratarse como un flujo de device.
Admin Process, la aplicación debe poder ejecutar como procesos independientes.

4. Tu primer Docker Swarm

Swarm no es un paquete aparte de docker, swarm es nativo.
Docker swarm init : inicia un nodo manager.
Debe existir como mínimo un manager para que exista swarm.
docker swarm join-token manager
docker node ls : muestra los nodos disponibles
Toda la comunicación entre nodos está encriptada usando certificados TLS.
docker node inspect --pretty self: inspecciona el nodo
docker swarm leave --force: cierra el modo swarm, regresa a docker normal.
https://docs.docker.com/engine/swarm/how-swarm-mode-works/pki/


5. Fundamentos de Docker Swarm: servicios

Un servicio puede agrupar a uno o varios contenedores.
docker service create --name nombre imagedocker command, para crear un nuevo servicio, con un nombre, imagen y comando específico en docker swarm.
docker service ls, para listar una serie de servicios.
Para poder crear un servicio docker swarm usa los mismos conceptos que se usa docker.






