
# Comandos DOCKER 
## *Listado de comandos utiles al momento de trabajar con contenedores e imagenes*.


`$ docker create nombre_imagen => Crea el CONTENEDOR a partir de una IMAGEN`

`$ docker container exec id_contenedor ls => Corre el contenedor e imprime el listado de archivos`

`$ docker container run nombre_imagen => Otra forma de correr el contenedor a partir de una imagen`

`$ docker container run -d nombre_imagen => Descarga la imagen y corre el contenedor en segundo plano`

`$ docker container run -dit nombre_imagen => Mantiene en segundo plano el stdinput abierto "el contenedor se queda corriendo"`

`$ docker container run -it nombre_imagen => Mantiene el contenedor corriendo y me lleva a la raiz del contenedor`

`$ docker container run -dit -v $PWD:/home/app -v /var/run/docker.sock:/var/run/docker.sock python:3.6 => Montamos un volumen con sockets para consumir la api dentro del contenedor`

`$ docker container exec -it id_contenedor bash => Corre la instancia del contenedor y me ingresa al BASH de la maquina`

`$ docker container exec -u 0 -it id_contenedor bash => Corre la instancia del contenedor y me ingresa como root al BASH`

`$ docker run -it nombre_imagen => Crea el contenedor y mantiene el stdinput abierto (interactivo)`

`$ docker run -d --name nombre_contenedor -p 8080:80(puertodelhost:puertocontenedor) nombre_imagen => Crea un contenedor al cual se accede por el host 80`

`$ docker ps -a => Lista todos los contenedores`

`$ ps -fa => Lista el proceso de todo el contenedor`

`$ docker ps -a => Lista todos los contenedores`

`$ docker ps -aq => Lista todos los contenedores por el id`

`$ docker inspect id_contenedor => Detalles internos del contenedor`

`$ docker inspect -f {{json details.details}} nombre_contenedor => Filtra una variable especifica del contenedor`

`$ docker rm nombre_contenedor => Elimina un contenedor`

`$ docker rm $(ps -aq) => Borra solo los contenedores que no estan corriendo`

`$ docker rm -f $(docker ps -aq) => Borra todos los contenedores incluso los que estan corriendo`

`$ docker rename nombre_contenedor "nombre_nuevo" => Le cambia el nombre del contenedor`

`$ docker container logs nombre_contenedor => Muestra los logs del contenedor`

`$ docker exec -it nombre_contenedor bash => Para ejecutar BASH dentro de un contenedor existente`

`$ docker kill nombre_contenedor => Mata el proceso completo del contenedor`

`$ docker rm -f nombre_contenedor => Forza la eliminación del contenedor`

`$ docker run -d --name nombre_contenedor -v /path/donde_esta/archivo:/path/donde/se_montara/contenedor nombre_imagen => Montar un sistema de archivos dentro del contenedor`

`$ docker volume ls => Lista todos los volumenes de cada contenedor`

`$ docker volume prune" => Borra todos los volumenes del contenedor`

`$ docker volume inspect "nombre_volumen" => Informacion del volumen`

`$ docker volume rm "nombre_volumen" => Elimina un volumen en especifico`
***

## Commits en DOCKER
*Creamos o modificamos una configuración en un sistema operativo y la guardamos con commit*

`$ docker container commit "container_id" "nombre_y_version_imagen" => Se transforma el contenedor de una imagen`
***

## Montar volumenes
*1. No es recomendable tener datos sensibles en los contenedores, por eso es necesaria la creación de volumenes externos que se puedan integrar facilmente con contenedores*

*2. De una misma forma un volumen se puede montar en diferentes contenedores y comunicarse entre sí*

`$ docker volume create "nombre_volumen" => Crea un volumen`

`$ docker volume ls => Lista los volumenes`

`$ docker volume inspect "nombre_volumen" => Información detallada del volumen`

`$ docker volume inspect "nombre_volumen" => Información detallada del volumen`

`$ docker container run -dit -v "nombre_volumen":/app nombre_imagen => Montamos el volumen en una ruta del contenedor`

***

## Imagenes
`$ docker images" => Lista las imagenes descargadas`

`$ docker run nombre_imagen => Descarga la imagen y la corre de una vez (esta es la mejor manera de descargar la imagen y correr el contenedor al mismo tiempo)`

`$ docker run --rm -w /app -v /path/en/nuestra/maquina/appNode:/app -it nombre_imagen bash => Descarga la imagen y corre el contenedor, cuando se cierra se autodestruye. Tambien establecemos directorio de trabajo dentro del contenedor y luego le pasamos un volumen del host al area de trabajo del contenedor y que ejecute un bash`

`$ docker run --rm -w /app -v /path/en/nuestra/maquina/appNode:/app -| it node npm install -s express => Se instala express dentro de mi pc local y dentro del contenedor`

`$ docker pull nombre_imagen => Descarga una imagen`

`$ docker rmi -f "imagen_id" => Elimina una imagen en especifico`
***

## Crear nuestras propias imagenes a partir de imagenes bases
*1. Creamos el archivo Dockerfile (comandos utiles)*

- COPY => Copia archivos o carpetas de una ruta al contenedor.
- ADD => Copia archivos o carpetas y nos permite descargar recursos o descomprimir archivos.
- ENV => Pasar variables de entorno del host al contenedor

*2. El achivo Dockerfile siempre debe empezar con el FROM y le indicamos nuestra imagen base.*

*3. Luego escribimos RUN para correr el comando y una accion para saber que se ha creado.*

*4. Construimos un Dockerfile con:*
`$ docker image build -t imagen:tag <path donde obtendremos el contexto build>`

*5. Una vez construida la imagen podemos correr el contenedor*

***

## Tags
`$ docker tag "tag_contenedor" "nuevo_tag_contenedor" => Actualiza el tag de un contenedor`

    $ docker tag ubuntu:jose <nexus>/ubuntu:jose

`$ docker push <nexus>/ubuntu:jose=> Hacer push a un contenedor`
***

## Redes
`$ docker network ls => Lista todas las redes`

`$ docker network rm nombre_red => Eliminar la red`

`$ docker network create --able nombre_red => Crea la red para que otros contenedores se conecten a esta red`

`$ docker network connect nombre_red nombre_contenedor => Conecta el contenedor a la red`

`$ docker network inspect nombre_red => Detalles de la red`

`$ docker network inspect -f '{{.Containers}}' nombre_red => Detalle especifico de la red`

`$ docker run -d --name nombre_contenedor -p 3000:3000 --env MONGO_URL=mongodb://db:27017/test --net=nombre_red nombre_imagen => Contenedor conecta a la red con una bd en mongo corriendo por el puerto 3000`

`$ docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 br02 => Creamos una red con ip estatica asignada al driver br02`

`$ docker container run -it --ip 10.1.0.10 --network br02 nombre_imagen => Asignamos ip estatica al contenedor`

***

## Docker Compose

`$ docker-compose up => Ejecuta un contenedor con toda su configuración`

`$ docker-compose up -d => Ejecuta un contenedor en segundo plano`

`$ docker-compose ps => Lista todos los contenedores creados`

`$ docker-compose logs nombre_servicio/contenedor => Logs del contenedor`

`$ docker-compose exec nombre_servicio bash => Entra al contenedor y ejecuta una terminal`

`$ docker-compose down => Borra todos los servicios y la red`

`$ docker-compose stop => Detiene los servicios y la red`

`$ docker-compose build => Contruir una imagen docker`

`$ docker-compose scale app-4 => Escala mi app (servicio) a 4 contenedores, lo recomendable es un solo puerto y que en el medio exista un distribuidor de carga`

***

## Docker Compose y Django
1. `$ export COMPOSE_FILE=local.yml`

2. `$ docker-compose run --rm django python manage.py createsuperuser => Creación de superusuario utilizando manage.py`

3. `$ docker-compose run --rm django python manage.py shell -i python => Shell de python con docker`

4. `$ docker-compose run --service-ports django => Corremos el servidor django`

***

## Entrypoint y CMD :
*En Docker, podemos construir imágenes a partir de archivos de texto plano conocidos como Dockerfiles.*

    1. El ENTRYPOINT especifica el ejecutable que usará el contenedor
        
        FROM ubuntu:16.04
        ENTRYPOINT ["/bin/echo"]
        CMD ["Hello"]
        Si usas la instrucción ENTRYPOINT, entonces Docker usará el ejecutable que le indiques, 
        y la instrucción CMD te permitirá definir un parámetro por defecto.

        
    2. y CMD se corresponde con los parámetros a usar con dicho ejecutable.

        FROM ubuntu:16.04
        CMD ["/bin/date"]
        El proceso del contenedor se iniciará con la ejecución de /bin/sh (por default) -c /bin/date.

        
## ¿Que sigue?
- Argumentos y variables de torno
- Proxy reversivo (traefik)
- KUBERNETES (minikube)
- Docker Service
***

#### Fuente
- https://codigofacilito.com
- https://platzi.com
- https://docker.com
- Diferentes recursos de internet