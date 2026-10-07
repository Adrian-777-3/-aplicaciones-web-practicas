# APARTADO 1

### Actualiza la lista de paquetes y el sistema:
- sudo ufw status       (ver si esta activo)
- sudo ufw allow        (Apache'permitir a apache trafico por causa del firewall)
### Comprueba la versión del sistema:
- lsb_release -a
  
![captura 1](fotos/Captura%201.png)

# APARTADO 2

### instalamos apache y comprobamos la version
- sudo apt install apache2 -y
### Comprueba la versión instalada:
- apache2 -v

![captura 1](fotos/Captura%204.png)

pregunta ¿Qué paquetes adicionales se han instalado como dependencias? (pista: revisa la salida de apt).

![captura 1](fotos/Captura%203.png)

todas las que pongan depended son opcionales 

# APARTADO 3

### comprobamos el estado del servidor 

sudo systemctl status apache2
### Un puerto en escucha es como una “puerta” por la que un servicio espera que otros equipos se conecten
sudo ss -tulpn | grep apache2

en Apache2:


- Puerto 80: permite acceder a páginas web mediante HTTP.
- Puerto 443: permite acceder a páginas web mediante HTTPS de forma cifrada.
- Puerto 22: permite conectarse al servidor remotamente mediante SSH.

### en apache2 Comprueba que Apache responde correctamente en el servidor local y muestra sus cabeceras HTTP

- curl -I http://localhost

![captura 1](fotos/Captura%205.png)

![captura 1](fotos/Captura%206.png)

### comprobamos si firewall esta activado y permitimos con el segundo comando conectar apache con firewall

- sudo ufw status        (Muestra si el cortafuegos del sistema está activado o apagado, y qué reglas de seguridad tiene configuradas en ese momento.)
- sudo ufw allow 'Apache'         (Añade una regla al cortafuegos para permitir el tráfico web no seguro por el puerto 80 hacia el servidor Apache)

pregunta ¿Qué diferencia hay entre los perfiles Apache, Apache Full y Apache Secure?

- Apache: permite solo HTTP (puerto 80).
- Apache Full: permite HTTP (80) y HTTPS (443).
- Apache Secure: permite solo HTTPS (443).
- puerto 433 conexion cifrada o segura y puerto 80 sin cifrado entrar a paginas web

# apartado 4

| Nº | Comando | Definición |
|---:|---|---|
| 1 | `sudo systemctl start apache2` | Enciende Apache y hace que empiece a funcionar el servidor. |
| 2 | `sudo systemctl stop apache2` | Apaga Apache y deja de funcionar el servidor. |
| 3 | `sudo systemctl restart apache2` | reinicia servidor apache (corta conexiones). |
| 4 | `sudo systemctl reload apache2` | recarga configuracion servidor apache (sin perder conexion) |
| 5 | `sudo systemctl enable apache2` | Hace que Apache se encienda automáticamente cuando arranque el ordenador. |
| 6 | `sudo systemctl disable apache2` | Hace que Apache no se encienda automáticamente al arrancar el ordenador. |
| 7 | `apache2ctl configtest` | Comprueba si la configuración de Apache está bien escrita y si hay errores. |
| 8 | `apache2ctl -S` | Muestra los sitios web (virtual host cargados) que Apache tiene configurados. |
| 9 | `apache2ctl -M` | Muestra los módulos de Apache que están activados. |
| 10 | `a2enmod / a2dismod` | Activa o desactiva módulos de Apache. Los módulos son funciones adicionales que puede utilizar Apache. |
| 11 | `a2ensite / a2dissite` | Activa o desactiva sitios web configurados en Apache. |
| 12 | `a2enconf / a2disconf` | Activa o desactiva configuraciones adicionales de Apache. |

### captura de cada comando

![captura 1](fotos/Captura%2022.png)

![captura 1](fotos/Captura%2023.png)

![captura 1](fotos/Captura%2024.png)

![captura 1](fotos/Captura%2025.png)

![captura 1](fotos/Captura%2026.png)

![captura 1](fotos/Captura%2027.png)

![captura 1](fotos/Captura%2028.png)

![captura 1](fotos/Captura%2029.png)

![captura 1](fotos/Captura%2030.png)

![captura 1](fotos/Captura%2031.png)

![captura 1](fotos/Captura%2032.png)

![captura 1](fotos/Captura%2033.png)

pregunta: ¿Cuándo conviene usar reload en lugar de restart?

Conviene usar reload cuando has cambiado la configuración de Apache y quieres que aplique los cambios sin apagar el servicio ni cortar las conexiones actuales.

# apartado 5

| Nº | Ruta | Descripción |
|---:|---|---|
| 1 | `/etc/apache2/apache2.conf` | Fichero de configuración principal. |
| 2 | `/etc/apache2/ports.conf` | Puertos en los que escucha Apache. |
| 3 | `/etc/apache2/sites-available/` | Sitios disponibles (definidos, no necesariamente activos). |
| 4 | `/etc/apache2/sites-enabled/` | Sitios activos (enlaces simbólicos a `sites-available`). |
| 5 | `/etc/apache2/mods-available/` y `mods-enabled/` | Módulos disponibles y activos. |
| 6 | `/etc/apache2/conf-available/` y `conf-enabled/` | Fragmentos de configuración disponibles y activos. |
| 7 | `/etc/apache2/envvars` | Variables de entorno (usuario y grupo de ejecución, etc.). |
| 8 | `/var/www/html/` | Directorio raíz por defecto (`DocumentRoot`). |
| 9 | `/var/log/apache2/access.log` | Registro de accesos. |
| 10 | `/var/log/apache2/error.log` | Registro de errores. |

### Comprobamos que los ficheros de sites-enabled son enlaces simbólicos y exploramos extructura de apache en /etc/apache2/

ls -l /etc/apache2/sites-enabled/

Ahí verás los archivos con una flecha como 000-default.conf -> ../sites-available/000-default.conf, indicando que son accesos directos (enlaces simbólicos)

Un elemento es un enlace simbólico si al ejecutar ls -l la línea empieza por l y muestra una flecha -> que apunta al archivo original.

![captura 1](fotos/Captura%2034.png)

# apartado 6

### copia seguridad servidor

![captura 1](fotos/Captura%208.png)

### cambiaremos su pagina de inicio

![captura 1](fotos/Captura%209.png)

### cambia el puerto de escucha 

Cambia Listen 80 por Listen 8080 y <VirtualHost *:80> por <VirtualHost *:8080>. Después:

![captura 1](fotos/Captura%2010.png)

![captura 1](fotos/Captura%2011.png)

- sudo apache2ctl configtest       (comprueba la sintaxis de los archivos de configuración del servidor web Apache para verificar que no contengan errores)

![captura 1](fotos/Captura%2012.png)

- sudo systemctl reload apache2 (recarga la configuración del servidor web Apache sin interrumpir las conexiones activas ni apagar el servicio.)

![captura 1](fotos/Captura%2013.png)

curl -I http://localhost:8080       (sirve para verificar que tu servidor web funciona bien y responde peticiones por la red.)

![captura 1](fotos/Captura%2014.png)

### Definir el nombre del servidor

![captura 1](fotos/Captura%2015.png)

###  Cambiar el correo del administrador por el tuyo

![captura 1](fotos/Captura%2017.png)

![captura 1](fotos/Captura%2016.png)

### Personalizar una página de error

Crear la página de error

![captura 1](fotos/Captura%2018.png)

añadimos informacion en el archivo 

![captura 1](fotos/Captura%2019.png)

Editar la configuración de Apache

![captura 1](fotos/Captura%2020.png)

![captura 1](fotos/Captura%2021.png)

# Apartado 7 Módulos de Apache





