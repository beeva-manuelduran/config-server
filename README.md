# config-server

Este proyecto nace por la necesidad de cambiar los ficheros de propiedades de un sistema distribuido sin necesidad de reiniciar servidores de aplicaciones. Como prueba de concepto se ha construido el siguiente esquema:

![PoC Config Server](https://github.com/beeva-manuelduran/config-server/blob/master/docs/images/magicboxteam_config-server.png?raw=true)

1. Realizamos los cambios en el fichero de propiedades y realizamos push al repositorio Git. Para este ejemplo, el repositorio Git está en [config-repo](https://github.com/beeva-manuelduran/config-repo).
2. El Config Server recibe estos cambios y actualiza el contexto. Estos cambios no se propagan automáticamente a las aplicaciones cliente, de modo que nos permite tener un control más estricto de cuándo queremos que empiecen a afectar los cambios realizados.
3. Llamamos al endpoint `/bus/refresh` que tenemos disponible en el Config Server.
4. El Config Server envía un mensaje de actualización disponible al Cloud Bus.
5. Una vez se envía el mensaje al Bus, los nodos:
  - a. reciben el mensaje de actualización disponible.
  - b. descargan la actualización y refrescan el entorno.


Este proyecto está dividido en tres repositorios:
* [config-server](https://github.com/beeva-manuelduran/config-server): Repositorio con el Config Server encargado de gestionar los cambios realizados en el repositorio de Git para actualizar las propiedades.
* [config-repo](https://github.com/beeva-manuelduran/config-repo): Repositorio con los ficheros de propiedades
* [config-client](https://github.com/beeva-manuelduran/config-client): Repositorio con un ejemplo de cliente conectado al Config Server y al 


## Frameworks

[Spring Cloud Config](http://cloud.spring.io/spring-cloud-config/) es un proyecto de Spring que nos permite externalizar la configuración en sistemas distribuidos. Nos facilita la creación de un Config Server para el control de actualizaciones y el refresco del contexto en las aplicaciones clientes. En la documentación oficial puedes encontrar más información de nomenclatura de ficheros de propiedades, conexiones con repositorios, etc.

[Spring Cloud Bus](http://projects.spring.io/spring-cloud/spring-cloud.html#_spring_cloud_bus) es un proyecto de Spring que nos permite unir nodos de un sistema distribuido a una cola de mensajes (RabbitMQ, SQS, etc)


## Configuración

Es necesario indicar las siguientes propiedades en el fichero `application.properties`:
- `spring.cloud.config.server.git.uri`: url del repositorio Git donde se encuentran los ficheros de propiedades. Más info [aquí](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html#_git_backend).
- `host`, `port`, `username` y `password` de la cola de mensajes. Más info [aquí](http://projects.spring.io/spring-cloud/spring-cloud.html#_spring_cloud_bus)

