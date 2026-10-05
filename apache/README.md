# APARTADO 1

### Actualiza la lista de paquetes y el sistema:
- sudo apt update
- sudo apt upgrade -y
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

### en Apache2 usaremos el comado curl -I http://localhost que Comprueba que Apache responde correctamente en el servidor local y muestra sus cabeceras HTTP

![captura 1](fotos/Captura%205.png)

![captura 1](fotos/Captura%206.png)

### comprobamos si firewall esta activado y permitimos con el segundo comando conectar apache con firewall

- sudo ufw status (UFW está inactivo, por lo que actualmente no está bloqueando las conexiones de Apache.)
- sudo ufw allow 'Apache'

pregunta ¿Qué diferencia hay entre los perfiles Apache, Apache Full y Apache Secure?

- Apache: permite HTTP (puerto 80).
- Apache Full: permite HTTP (80) y HTTPS (443).
- Apache Secure: permite solo HTTPS (443).
- puerto 433 conexion cifrada o segura y puerto 80 sin cifrado entrar a paginas web

# apartado 4

|sudo systemctl start apache2 | Enciende Apache y hace que empiece a funcionar.|
--------------------------------------------------------------------------------
|sudo systemctl stop apache2 | Apaga Apache y deja de funcionar.|
-
|sudo systemctl restart apache2 | Apaga y vuelve a encender Apache. Puede cortar las conexiones que estén activas.|
|sudo systemctl reload apache2 | Hace que Apache vuelva a leer su configuración, pero sin apagarlo ni cortar las conexiones.|
|sudo systemctl enable apache2 | Hace que Apache se encienda automáticamente cuando arranque el ordenador.|
|sudo systemctl disable apache2 | Hace que Apache no se encienda automáticamente al arrancar el ordenador.|
|apache2ctl configtest | Comprueba si la configuración de Apache está bien escrita y si hay errores.|
|apache2ctl -S | Muestra los sitios web que Apache tiene configurados.|
|apache2ctl -M | Muestra los módulos de Apache que están activados.|
|a2enmod / a2dismod | Activa o desactiva módulos de Apache. Los módulos son funciones adicionales que puede utilizar Apache.|
|a2ensite / a2dissite | Activa o desactiva sitios web configurados en Apache.|
a2enconf / a2disconf | Activa o desactiva configuraciones adicionales de Apache.|

