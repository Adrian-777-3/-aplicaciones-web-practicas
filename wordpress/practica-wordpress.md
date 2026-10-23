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

```sql
CREATE DATABASE wordpress;
```
Crea la base de datos donde WordPress guardará su información.

### 3.2. Crear el usuario de WordPress

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



































