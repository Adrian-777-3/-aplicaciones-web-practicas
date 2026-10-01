# APARTADO 1

### Actualiza la lista de paquetes y el sistema:
- sudo apt update
- sudo apt upgrade -y
### Comprueba la versión del sistema:
- lsb_release -a
  
![captura 1](fotos/Captura%201.png)

# APARTADO 2

### instalamos apache y comprobamos la version
-sudo apt install apache2 -y
### Comprueba la versión instalada:
-apache2 -v

![captura 1](fotos/Captura%204.png)

preguntas ¿Qué paquetes adicionales se han instalado como dependencias? (pista: revisa la salida de apt).

![captura 1](fotos/Captura%203.png)

# APARTADO 3

### comprobamos el estado del servidor 
sudo systemctl status apache2
### Un puerto en escucha es como una “puerta” por la que un servicio espera que otros equipos se conecten
sudo ss -tulpn | grep apache2

en Apache2:
- Puerto 80: permite acceder a páginas web mediante HTTP.
- Puerto 443: permite acceder a páginas web mediante HTTPS de forma cifrada.
- Puerto 22: permite conectarse al servidor remotamente mediante SSH.




