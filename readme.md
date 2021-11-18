# **Creación de contenedores con Docker Compose**
***
Docker Compose permite arrancar de forma simultanea varios contenedores a traves de un unico fichero de configuracion. Las ventajas de este formato son evidentes, pues facilitan la portabilidad y la automatizacion de las instalaciones. Eso si, el fichero de configuracion debe de estar perfectamente escrito, extremando el cuidado a la hora de identar y estructurar las lineas. 

Para levantar contenedores utilizando Docker Compose solamente deberemos descargar el Docker-Compose.yml, acceder a la ruta en la que hayamos almacenado el archivo y ejecutar

```
docker compose up
```

Si queremos sobreescribir el comando definido en la configuración para uno de los servicios deberemos usar el comando run en lugar de app. Es decir, si tuvieramos un Docker Compose con un contenedor llamado web y en el archivo de configuración indicamos que arranque una terminal nosotros podriamoos sobreescribir esa indicacion

```
docker-compose run web python app.py
```

Con esto el contenedor iniciaria, en caso de que lo soportase, con el servicio python app.py en lugar de la terminal bash.

# **Configuración de Bind9**

Para poder acceder a los archivos de Bind9 y establecer la configuración deseada para nuestro servidor DNS podemos acceder al contenedor a través de Visual Estudio Code. Entramos en la pestaña de docker, buscamos nuestro conjunto de contenedores y pulsamos sobre la flecha que hay a la izquierda del nombre de nuestro contenedor DNS. Asi accederemos al sistema de archivos.

Otra opcion, como hemos configurado el contenedor para que los ficheros de configuración formen un volumen, es decir, que ocupan un lugar reservado gestionado por docker, de forma que si queremos podemos compartirlos con varios contenedores y realizar una sola modificacion que afecte a todos ellos, es acceder directamente al volumen y realizar la modificacion de esa manera. Para ello debemos acceder al volumen pulsando sobre él con click derecho y seleccionando "explore in a development container". Es posible que para ello debamos instalar la extension "Remote Containers" de Visual Code si no la tenemos ya instalada.

Los cambios en la configuración de Bind9 se detallan en el otro repositorio de la practica. Una vez realizados estos cambios debemos reiniciar el servicio para que tengan efecto. Sin embargo, no se puede reiniciar el servicio utilizando los comandos del SO, puesto que Docker funciona de otra manera. Un contenedor Docker se levanta especificamente para ejecutar un servicio determinado (de ahi su ligereza y rapidez, ya que contiene lo imprescindible para ejecutar ese servicio y toma el resto de recursos necesarios del SO anfitrion) y ese servicio no se puede detener sin reiniciar el propio contenedor. Es decir, el contenedor **_es_** el propio servicio, de forma que no tiene sentido que el contenedor siga funcionando en el momento en el que ese servicio se detiene.

Por tanto, para que tengan efecto los cambios de configuracion que requieran reiniciar un servicio debemos realizar un reinicio del contenedor.