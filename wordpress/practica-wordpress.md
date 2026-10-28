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































