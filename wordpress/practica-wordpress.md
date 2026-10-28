# Práctica: Instalación de WordPress en Ubuntu 24.04

**Alumno:** Adrián Pérez Cereceda  
**Curso:** 2.º SMX  
**Módulo:** Aplicaciones Web  
**Fecha:** 23/10/2026

---

## Apartado 1. Conexión por SSH y comprobación del servidor

### 1.1. Averiguar la IP del servidor

Ejecutamos el siguiente comando para conocer las direcciones IP de nuestro servidor Ubuntu:

```bash
hostname -I
```

El servidor tiene las direcciones IPv4 `10.0.2.15` y `192.168.56.24`, además de una dirección IPv6. Utilizaremos la dirección `192.168.56.24` para acceder desde el ordenador anfitrión.

### 1.2. Conectarse por SSH

Nos conectamos al servidor mediante SSH para administrarlo desde la terminal del ordenador.

### 1.3. Comprobar los servicios y la versión de PHP

Comprobamos que Apache y MariaDB están funcionando correctamente:

```bash
systemctl is-active apache2 mariadb
```

Después, comprobamos la versión de PHP instalada:

```bash
php -v
```

### Captura de la conexión SSH y comprobación del servidor

![Comprobación del servidor](fotos/Captura%201.png)

### Pregunta:

¿Qué dos "puertas" (programas y puertos) usas para trabajar con el servidor en esta práctica y para qué sirve cada una?

SSH utiliza el puerto 22 y permite administrar el servidor mediante una terminal. HTTP utiliza el puerto 80 y permite acceder a WordPress desde el navegador.

## Apartado 2. Dejar Apache con un único sitio

### 2.1. Cambiar los sitios activos

Desactivamos los sitios virtuales anteriores y activamos el sitio predeterminado de Apache.

```bash
sudo a2dissite sitio1.conf sitio2.conf
```
Desactiva los sitios virtuales `sitio1` y `sitio2` sin borrar sus archivos.

```bash
sudo a2ensite 000-default.conf
```
Activa el sitio predeterminado de Apache.

```bash
sudo apache2ctl configtest
```
Comprueba si la configuración de Apache tiene errores.

```bash
sudo systemctl reload apache2
```
Recarga Apache para aplicar los cambios de configuración.

### 2.2. Comprobar los sitios activos y la carpeta raíz

```bash
ls /etc/apache2/sites-enabled/
```
Muestra los sitios virtuales habilitados en Apache.

```bash
grep DocumentRoot /etc/apache2/sites-enabled/000-default.conf
```
Muestra la carpeta raíz que utiliza el sitio predeterminado.

### Captura de los comandos y sus resultados:

![Comprobación de sitios activos](fotos/Captura%202.png)

### 2.3. Comprobar el acceso desde el navegador

Abrimos `http://192.168.56.24` en el navegador para comprobar que podemos acceder al servidor web.

### Captura de la página de Apache en el navegador:

![Página de Apache en el navegador](fotos/Captura%203.png)

### Pregunta:

¿Qué diferencia hay entre desactivar un sitio con `a2dissite` y borrar sus ficheros? ¿Por qué ahora ves la página por defecto de Apache y antes veías sitio1?

Desactivar un sitio impide que Apache lo utilice, pero sus archivos siguen guardados en el servidor. 

## Apartado 3. Crear la base de datos de WordPress

### 3.1. Crear la base de datos

| Qué creamos | Nombre | Para qué |
|---|---|---|
| El **armario** (base de datos) | `wordpress` | Donde WordPress guardará todos sus datos. |
| La **llave** (usuario) | `wp_adrian` | Con la que WordPress se conectará a la base de datos. |
| El **permiso** | Todos los permisos sobre `wordpress` | Esa llave puede gestionar la base de datos `wordpress`, pero no otras bases de datos. |


```sql
CREATE DATABASE wordpress;
```
Crea la base de datos donde WordPress guardará su información.

### 3.2. Crear el usuario de WordPress

| Orden | Qué hace |
|---|---|
| `CREATE DATABASE wordpress;` | Crea el armario, es decir, la base de datos. |
| `CREATE USER 'wp_adrian'@'localhost' IDENTIFIED BY '...';` | Crea la llave y le pone una contraseña. |
| `GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_adrian'@'localhost';` | Da permiso al usuario para gestionar la base de datos `wordpress`. |


```sql
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY '********';
```
Crea un usuario para gestionar la base de datos de WordPress.

```sql
ALTER USER 'wp_adrian'@'localhost' IDENTIFIED BY '********';
```
Establece una contraseña para el usuario `wp_adrian`. La contraseña real no se publica en GitHub.

### 3.3. Asignar permisos

```sql
GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_adrian'@'localhost';
```
Concede al usuario permisos sobre la base de datos `wordpress`.

### 3.4. Comprobar el acceso

```bash
mariadb -u wp_adrian -p wordpress
```
Comprueba que el usuario puede conectarse a la base de datos. La contraseña se introduce cuando la solicita el sistema.

### Captura de la comprobación de la base de datos

![Comprobación de la base de datos de WordPress](fotos/Captura%205.png)

### Pregunta:

¿Por qué es recomendable crear un usuario exclusivo para WordPress?

Porque permite gestionar su propia base de datos sin utilizar directamente la cuenta administradora de MariaDB.

## 4. Descargar e instalar los ficheros de WordPress

### 4.1. Descargar y descomprimir WordPress

Instalamos `wget`, nos situamos en la carpeta temporal y descargamos WordPress. Después, descomprimimos el archivo y comprobamos su contenido.

```bash
sudo apt install wget -y
```

```bash
cd /tmp
```

```bash
wget https://wordpress.org/latest.tar.gz
```

```bash
tar -xzf latest.tar.gz
```

```bash
ls /tmp/wordpress
```

**Explicación de los comandos:**

- `apt install wget -y`: instala la herramienta para descargar archivos.
- `cd /tmp`: entra en la carpeta temporal.
- `wget`: descarga WordPress desde su página oficial.
- `tar -xzf`: descomprime el archivo descargado.
- `ls`: muestra los archivos de la carpeta.

### 4.2. Copiar WordPress al servidor web

Movemos el contenido de WordPress al directorio principal de Apache. En nuestro caso, conservamos la página anterior como copia de seguridad y copiamos los archivos de WordPress.

```bash
sudo mv /var/www/html/index.html /var/www/html/index.html.bak
```

```bash
sudo cp -a /tmp/wordpress/. /var/www/html/
```

```bash
ls /var/www/html/
```

**Explicación de los comandos:**

- `mv`: cambia el nombre de la página anterior para conservarla como copia de seguridad.
- `cp -a`: copia los archivos de WordPress manteniendo sus atributos.
- `ls`: comprueba que los archivos se encuentran en el directorio web.

**Pregunta:** Explica qué hace cada uno de los comandos que acabas de ejecutar.

**Respuesta:** Los comandos permiten descargar WordPress, descomprimirlo, conservar la página anterior y copiar los archivos al directorio web de Apache. Finalmente, comprobamos que los archivos se han copiado correctamente.

### 4.3. Configurar los permisos

Asignamos los archivos al usuario y grupo `www-data`, que utiliza Apache, y configuramos los permisos de directorios y archivos.

```bash
sudo chown -R www-data:www-data /var/www/html
```

```bash
sudo find /var/www/html -type d -exec chmod 755 {} \;
```

```bash
sudo find /var/www/html -type f -exec chmod 644 {} \;
```

```bash
ls -ld /var/www/html
```

**Explicación de los comandos:**

- `chown -R`: cambia el propietario y el grupo de los archivos y directorios.
- `find ... -type d ... chmod 755`: establece permisos `755` en los directorios.
- `find ... -type f ... chmod 644`: establece permisos `644` en los archivos.
- `ls -ld`: muestra los permisos, el propietario y el grupo del directorio principal.

### Pregunta:
¿Por qué hay que cambiar el propietario de la carpeta de WordPress a `www-data`? ¿Qué podría fallar si siguiera siendo tu usuario?

 Porque Apache utiliza el usuario `www-data` para trabajar con los archivos de la web. Si los permisos no fueran adecuados, WordPress podría tener problemas al guardar imágenes, crear su archivo de configuración o instalar actualizaciones y complementos.

### Captura de la carpeta de WordPress y sus permisos

![Archivos de WordPress](fotos/Captura%206.png)

### 5.3. Iniciar sesión en WordPress

Una vez finalizada la instalación, iniciamos sesión con el usuario y la contraseña del administrador para acceder al panel de WordPress.

WordPress sirve para crear y administrar páginas web y blogs sin tener que programar toda la web desde cero. Desde el panel podemos publicar entradas, añadir páginas, subir imágenes y personalizar el sitio web.

### Captura 7. Inicio de sesión en WordPress

![Inicio de sesión en WordPress](fotos/Captura%207.png)

### Captura 8. Panel de administración de WordPress

![Panel de administración de WordPress](fotos/Captura%208.png)

### Pregunta:

¿Qué partes de la instalación has hecho en la terminal y cuáles en el navegador? ¿Por qué el servidor no necesita tener interfaz gráfica?

La terminal se ha utilizado para configurar el servidor y preparar WordPress. En el navegador hemos terminado la instalación e iniciado sesión. El servidor no necesita interfaz gráfica porque podemos administrarlo desde el navegador de otro equipo.

## Apartado 6. Comprobamos que WordPress funciona

### 6.1. Comprobar la web pública

Abro `http://192.168.56.24` en el navegador y compruebo que aparece mi página de WordPress.

### Captura 9. Web pública de WordPress

![Web pública de WordPress](fotos/Captura%209.png)

### 6.2. Acceder al panel de administración

Entro en `http://192.168.56.24/wp-admin` e inicio sesión con mi usuario y contraseña de WordPress.

### 6.3. Publicar una entrada de prueba

Desde el panel de administración, entro en **Entradas → Añadir nueva**, escribo un título y un texto breve y pulso **Publicar**. Después, compruebo que la entrada aparece en la web pública.

### Captura 10. Entrada publicada

![Entrada publicada en WordPress](fotos/Captura%2010.png)

### 6.4. Comprobar las tablas de WordPress

Ejecuto este comando en la terminal para mostrar las tablas de la base de datos:

```bash
sudo mariadb wordpress -e "SHOW TABLES;"
```

El comando muestra las tablas creadas por WordPress. Entre ellas está `wp_posts`, donde se guardan las entradas y páginas.

### Captura 11. Tablas de WordPress

![Tablas de WordPress en MariaDB](fotos/Captura%2011.png)

### Pregunta:

¿Dónde guarda WordPress las entradas que escribes: en ficheros del servidor o en la base de datos? ¿Cómo lo has comprobado?

WordPress guarda las entradas en la base de datos. Lo he comprobado ejecutando el comando `SHOW TABLES` y viendo la tabla `wp_posts`.

## Apartado 7. Configuración de la URL de WordPress

### 7.1. Modificar el archivo de configuración

Abro el archivo de configuración de WordPress:

```bash
sudo nano /var/www/html/wp-config.php
```

Añado estas dos líneas justo antes del comentario `That's all, stop editing!`:

```php
define( 'WP_HOME', 'http://' . $_SERVER['HTTP_HOST'] );
define( 'WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] );
```

Estas líneas hacen que WordPress utilice el nombre de host o la IP con la que se accede a la web.

### 7.2. Comprobar la sintaxis

Ejecuto el siguiente comando para comprobar que el archivo no contiene errores de sintaxis:

```bash
php -l /var/www/html/wp-config.php
```

El resultado indica que no hay errores de sintaxis.

### Captura 12. Comprobación del archivo de configuración

![Comprobación de wp-config.php](fotos/Captura%2012.png)

### Pregunta:

¿Por qué ayuda esta configuración si cambia la IP del servidor?

WordPress utiliza el host con el que se accede a la web, en lugar de tener una IP fija escrita en esas dos líneas. Así, en esta práctica, puede adaptarse a una IP diferente al acceder desde el navegador.

## Apartado 8. Dos ajustes para trabajar cómodamente

### 8.1. Direcciones bonitas (enlaces permanentes)

Creo el archivo de configuración de Apache:

```bash
sudo nano /etc/apache2/conf-available/wordpress.conf
```

Añado esta configuración para permitir que Apache lea el archivo `.htaccess`:

```apache
<Directory /var/www/html>
    AllowOverride All
</Directory>
```

Activo el módulo `rewrite`:

```bash
sudo a2enmod rewrite
```

Activo la configuración de WordPress en Apache:

```bash
sudo a2enconf wordpress
```

Compruebo que la configuración no tiene errores:

```bash
sudo apache2ctl configtest
```

Reinicio Apache para aplicar los cambios:

```bash
sudo systemctl restart apache2
```

Después, entro en **Ajustes → Enlaces permanentes**, selecciono **Nombre de la entrada** y guardo los cambios. Compruebo que una entrada se abre correctamente con su nueva dirección.

Verifico que existe el archivo `.htaccess`:

```bash
ls -la /var/www/html/.htaccess
```

### Captura 14. Configuración de Apache

![Configuración de Apache](fotos/Captura%2014.png)

### Captura 15. Enlaces permanentes de WordPress

![Enlaces permanentes de WordPress](fotos/Captura%2015.png)

### 8.2. Aumentar el tamaño máximo de subida

Configuro PHP para permitir subir archivos de hasta 64 MB:

```bash
printf 'upload_max_filesize = 64M\npost_max_size = 64M\n' | sudo tee /etc/php/8.3/apache2/conf.d/99-wordpress.ini
```

Reinicio Apache para aplicar los cambios:

```bash
sudo systemctl restart apache2
```

Entro en **Medios → Añadir nuevo** y compruebo que el tamaño máximo de subida es de 64 MB.

### Captura 16. Tamaño máximo de subida

![Tamaño máximo de subida de WordPress](fotos/Captura%2016.png)

### Pregunta:

¿Por qué las direcciones "bonitas" dan error 404 si Apache no tiene permitido leer el fichero `.htaccess`?

Porque Apache no puede leer las reglas de reescritura del archivo `.htaccess`. Por eso, no interpreta correctamente la dirección de la entrada y puede mostrar un error 404.

## Apartado 9. ¿Qué pasa si MariaDB se para?

WordPress necesita MariaDB para acceder a la base de datos donde guarda las entradas, páginas y ajustes. Para comprobarlo, detengo el servicio temporalmente.

### 9.1. Detener MariaDB

Ejecuto el siguiente comando para detener el servicio:

```bash
sudo systemctl stop mariadb
```

Después, abro `http://192.168.56.24` en el navegador. WordPress muestra un error de conexión con la base de datos porque MariaDB está detenido.

### Captura 17. Error de conexión con la base de datos

![Error de conexión con MariaDB](fotos/Captura%2017.png)

### 9.2. Volver a iniciar MariaDB

Arranco de nuevo el servicio:

```bash
sudo systemctl start mariadb
```

Compruebo que está funcionando:

```bash
systemctl is-active mariadb
```

El resultado es `active`, por lo que MariaDB está activo. Finalmente, recargo la web y compruebo que WordPress vuelve a funcionar.

### Captura 18. WordPress recuperado

![WordPress funcionando después de iniciar MariaDB](fotos/Captura%2018.png)

### Pregunta:

Cuando WordPress muestra "Error al establecer una conexión con la base de datos", ¿qué dos cosas comprobarías y en qué orden?

1. Comprobaría si MariaDB está funcionando con `systemctl is-active mariadb`. Si está detenido, lo iniciaría con `sudo systemctl start mariadb`.
2. Si MariaDB está activo y el error continúa, comprobaría que el usuario, la contraseña y el nombre de la base de datos configurados en WordPress son correctos.


## Apartado 10. Comprobación final

### 10.1. Comprobar el estado de los servicios

Ejecuto el siguiente comando para comprobar que Apache y MariaDB están funcionando:

```bash
systemctl is-active apache2 mariadb
```

Los dos servicios muestran el estado `active`.

### 10.2. Comprobar la versión de PHP

Ejecuto el siguiente comando:

```bash
php -v
```

El resultado muestra que tengo instalada la versión PHP 8.3.6.

### 10.3. Comprobar el sitio habilitado de Apache

Ejecuto este comando:

```bash
ls /etc/apache2/sites-enabled/
```

El resultado muestra que el único sitio habilitado es `000-default.conf`.

### 10.4. Comprobar las tablas de WordPress

Ejecuto el siguiente comando para mostrar las tablas de la base de datos `wordpress`:

```bash
sudo mariadb wordpress -e "SHOW TABLES;"
```

El resultado muestra las tablas creadas por WordPress, entre ellas `wp_posts`, `wp_users` y `wp_options`.

### 10.5. Resumen final de la instalación

- **Linux:** Ubuntu 24.04, instalado.
- **Apache:** Apache2, con un único sitio habilitado (`000-default.conf`), funcionando.
- **MariaDB:** versión 10.11, funcionando y con la base de datos `wordpress`.
- **PHP:** versión 8.3.6, instalada.
- **WordPress:** instalado en `/var/www/html` y accesible desde el navegador.

### Captura 19. Comprobación final de WordPress

![Comprobación final de Apache, MariaDB, PHP y las tablas de WordPress](fotos/Captura%2019.png)


















