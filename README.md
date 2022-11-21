# Lamp Configuración
Un stack LAMP construido con Docker Compose, conteine lo siguiente:
* PHP
* Apache
* MySQL
* phpMyAdmin
* Redis


Dispone de las siguientes versiones de PHP
* 5.4.x
* 5.6.x
* 7.1.x
* 7.2.x
* 7.3.x
* 7.4.x

> Considere que se ha simplificado la estructura de proyecto de varias ramas para cada ersión de php a una rama centralizada.

##  Instalación
* Clona este repositorio en una carpeta local.
* Configure el archivo .env como necesite.
* ejecute ```docker-compose up -d```


```shell
git clone
cd docker-compose-lamp/
cp sample.env .env
// modify sample.env as needed
docker-compose up -d
// visitar localhost
```
Puede visitar el sitio mediante `http://localhost`

##  Configuración y uso

### Información general
Este docker stack está construido para el desarrollo local y no para el uso de producción

### Configuración
Este paquete viene con opciones de configuración por defecto. Puedes modificarlas creando un archivo `.env` en tu directorio raíz.
Para hacerlo más fácil, simplemente copie el contenido del archivo `sample.env` y actualice los valores de las variables de entorno según sus necesidades.

### Variables de configuración.
Hay las siguientes variables de configuración disponibles y puedes personalizarlas sobrescribiendo en tu propio archivo `.env`.


---
#### PHP
---
_**PHPVERSION**_
Se utiliza para especificar la versión de PHP que desea utilizar. Por defecto siempre se utiliza la última versión de PHP. 

_**PHP_INI**_
Defina su modificación personalizada de `php.ini` para satisfacer sus requerimientos. 

---
#### Apache 
---

_**DOCUMENT_ROOT**_
Es la raíz del documento para el servidor Apache. El valor por defecto es `./www`. Todos sus sitios irán aquí y se sincronizarán automáticamente.

_**VHOSTS_DIR**_
Esto es para los hosts virtuales. El valor por defecto es `./config/vhosts`. Puede colocar sus archivos conf de hosts virtuales aquí.

> Asegúrese de añadir una entrada al archivo `hosts` de su sistema para cada host virtual.

_**APACHE_LOG_DIR**_
Se utilizará para almacenar los registros de Apache. El valor por defecto es `./logs/apache2`.

---
#### Database
---

_**DATABASE**_
Defina qué versión de MySQL o MariaDB desea utilizar. 

_**MYSQL_DATA_DIR**_
Este es el directorio de datos de MySQL. El valor por defecto es `./data/mysql`. Todos sus archivos de datos de MySQL se almacenarán aquí.

_**MYSQL_LOG_DIR**_
Se utilizará para almacenar los registros de Apache. El valor por defecto es `./logs/mysql`.

## Web Server
Apache está configurado para funcionar en el puerto 80. Por lo tanto, se puede acceder a través de `http://localhost`.

#### Apache Modules
Por defecto, los siguientes módulos están activados.
* rewrite
* headers
> Si quieres habilitar más módulos, sólo tienes que actualizar `./bin/webserver/Dockerfile`. También puedes generar un PR y lo fusionaremos si parece bueno para el propósito general.

> Tienes que reconstruir la imagen docker ejecutando `docker-compose build` y reiniciar los contenedores docker.

#### Connect via SSH
Puedes conectarte al servidor web usando el comando `docker-compose exec` para realizar varias operaciones en él. Utilice el siguiente comando para iniciar sesión en el contenedor a través de ssh.

```shell
docker-compose exec webserver bash
```

## PHP
La versión instalada de depende de su archivo `.env`. 
#### Extensions
Por defecto se instalan las siguientes extensiones. 
Puede ser diferente para las versiones de PHP <7.x.x
* mysqli
* pdo_sqlite
* pdo_mysql
* mbstring
* zip
* intl
* mcrypt
* curl
* json
* iconv
* xml
* xmlrpc
* gd

> Si quieres instalar más extensiones, sólo tienes que actualizar `./bin/webserver/Dockerfile`. También puedes generar un PR y lo fusionaremos si parece bueno para el propósito general.

> Tienes que reconstruir la imagen docker ejecutando `docker-compose build` y reiniciar los contenedores docker.

## phpMyAdmin

phpMyAdmin está configurado para ejecutarse en el puerto 8080. Utilice las siguientes credenciales por defecto.
```
http://localhost:8080/  
username: root  
password: docker
```

## Redis

Viene con Redis. Se ejecuta en el puerto por defecto `6379`.

## Contribuyendo a
Estamos contentos si quieres crear un pull request o ayudar a la gente con sus problemas. Si desea crear un PR, por favor recuerde que esta pila no está construida para el uso de producción, y los cambios deben ser buenos para el propósito general y no demasiado especializados.

> Por favor, tenga en cuenta que hemos simplificado la estructura del proyecto de varias ramas para cada versión de php, a una rama maestra centralizada.  Por favor, cree su PR contra la rama maestra

Traducción realizada con la versión gratuita del traductor www.DeepL.com/Translatore centralized master branch.  Please create your PR against master branch. 

Muchas gracias. 

## Por qué no deberías usar este stack sin modificar en producción
La idea es que los desarrolladores puedan crear rápidamente aplicaciones creativas. Por lo tanto, estoy proporcionando un entorno de desarrollo local fácil de configurar para varios Frameworks y versiones de PHP diferentes.

En Producción deberá modificar como mínimo los siguientes temas:
* php handler: mod_php => php-fpm
* asegurar los usuarios de mysql con las limitaciones de IP de origen adecuadas