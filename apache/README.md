



<div align="center">

# 🌐 Servidor Web Apache

### Instalación, configuración y securización en Ubuntu 24.04

**Alumno:** Adrián Pérez Cereceda

**Curso:** 2.º SMX — Sistemas Microinformáticos y Redes

**Módulo:** Aplicaciones Web

**Fecha:** 14/10/2026

🖥️ Ubuntu Server · ⚙️ Apache2 · 🔒 Seguridad

---

</div>



## 📑 Índice

- [ Apartado 1. Preparación del sistema](#apartado-1)
- [ Apartado 2. Instalación de Apache](#apartado-2)
- [ Apartado 3. Comprobación del funcionamiento](#apartado-3)
- [ Apartado 4. Comandos de administración](#apartado-4)
- [ Apartado 5. Ficheros y directorios importantes](#apartado-5)
- [ Apartado 6. Modificaciones del servicio](#apartado-6)
- [ Apartado 7. Módulos de Apache](#apartado-7-módulos-de-apache)
- [ Apartado 8. Virtual Hosts](#apartado-8-varios-sitios-en-un-mismo-servidor-virtual-hosts)
- [ Apartado 9. Permisos y propietarios](#apartado-9-permisos-y-propietarios)
- [ Apartado 10. Conclusiones](#apartado-10-conclusion)

---

# APARTADO 1

### Actualiza la lista de paquetes y el sistema:
```bash
- sudo ufw status       (ver si esta activo)
- sudo ufw allow        (Apache'permitir a apache trafico por causa del firewall)
```
### Comprueba la versión del sistema:
```bash
- lsb_release -a
```
![captura 1](fotos/Captura%201.png)

# APARTADO 2

### instalamos apache y comprobamos la version
```bash
- sudo apt install apache2 -y
```
### Comprueba la versión instalada:
```bash
- apache2 -v
```
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
```bash
- curl -I http://localhost
```
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

| Nº | Comando | Definición | Captura |
|---:|---|---|---|
| 1 | `sudo systemctl start apache2` | Inicia Apache y pone en funcionamiento el servidor web. | ![Captura 22](fotos/Captura%2022.png) |
| 2 | `sudo systemctl stop apache2` | Detiene Apache y apaga el servidor web. | ![Captura 23](fotos/Captura%2023.png) |
| 3 | `sudo systemctl restart apache2` | Reinicia Apache y puede interrumpir temporalmente las conexiones. | ![Captura 24](fotos/Captura%2024.png) |
| 4 | `sudo systemctl reload apache2` | Recarga la configuración sin reiniciar completamente el servicio. | ![Captura 25](fotos/Captura%2025.png) |
| 5 | `sudo systemctl enable apache2` | Configura Apache para iniciarse automáticamente al arrancar el ordenador. | ![Captura 26](fotos/Captura%2026.png) |
| 6 | `sudo systemctl disable apache2` | Evita que Apache se inicie automáticamente al arrancar el ordenador. | ![Captura 27](fotos/Captura%2027.png) |
| 7 | `apache2ctl configtest` | Comprueba si la configuración de Apache contiene errores de sintaxis. | ![Captura 28](fotos/Captura%2028.png) |
| 8 | `apache2ctl -S` | Muestra los sitios virtuales (Virtual Hosts) configurados en Apache. | ![Captura 29](fotos/Captura%2029.png) |
| 9 | `apache2ctl -M` | Muestra los módulos de Apache cargados. | ![Captura 30](fotos/Captura%2030.png) |
| 10 | `a2enmod / a2dismod` | Activa o desactiva módulos de Apache. | ![Captura 31](fotos/Captura%2031.png) |
| 11 | `a2ensite / a2dissite` | Activa o desactiva sitios web configurados en Apache. | ![Captura 32](fotos/Captura%2032.png) |
| 12 | `a2enconf / a2disconf` | Activa o desactiva configuraciones adicionales de Apache. | ![Captura 33](fotos/Captura%2033.png) |


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
```bash
ls -l /etc/apache2/sites-enabled/
```
Ahí verás los archivos con una flecha como 000-default.conf -> ../sites-available/000-default.conf, indicando que son accesos directos (enlaces simbólicos)

Un elemento es un enlace simbólico si al ejecutar ls -l la línea empieza por l y muestra una flecha -> que apunta al archivo original.

![captura 1](fotos/Captura%2034.png)

# apartado 6

### copia seguridad servidor
```bash
sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak
```
![captura 1](fotos/Captura%208.png)

### cambiaremos su pagina de inicio
```bash
echo "<h1>Servidor de Adrian</h1>" | sudo tee /var/www/html/index.html
```
![captura 1](fotos/Captura%209.png)

### cambia el puerto de escucha 
```bash
sudo nano /etc/apache2/ports.conf
```
Cambia Listen 80 por Listen 8080 y <VirtualHost *:80> por <VirtualHost *:8080>. Después:

![captura 1](fotos/Captura%2010.png)

![captura 1](fotos/Captura%2011.png)
```bash
- sudo apache2ctl configtest       (comprueba la sintaxis de los archivos de configuración del servidor web Apache para verificar que no contengan errores)
```
```bash
![captura 1](fotos/Captura%2012.png)
```
```bash
- sudo systemctl reload apache2 (recarga la configuración del servidor web Apache sin interrumpir las conexiones activas ni apagar el servicio.)
```

![captura 1](fotos/Captura%2013.png)
```bash
curl -I http://localhost:8080       (sirve para verificar que tu servidor web funciona bien y responde peticiones por la red.)
```

![captura 1](fotos/Captura%2014.png)

### Definir el nombre del servidor
```bash
echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/servername.conf
```
![captura 1](fotos/Captura%2015.png)

###  Cambiar el correo del administrador por el tuyo
```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```
![captura 1](fotos/Captura%2017.png)

![captura 1](fotos/Captura%2016.png)

### Personalizar una página de error
```bash
sudo nano /var/www/html/404.html
```
Crear la página de error

![captura 1](fotos/Captura%2018.png)

añadimos informacion en el archivo 

![captura 1](fotos/Captura%2019.png)

Editar la configuración de Apache

![captura 1](fotos/Captura%2020.png)

![captura 1](fotos/Captura%2021.png)

# Apartado 7 Módulos de Apache

apache es un núcleo pequeño (gestiona conexiones, lee la configuración, arranca procesos) pero se le pueden añadir mas modulos con mas funciones para evitar mas consumo de recursos (centrandose en solo modulos activos)

### que es un modulo y porque es importante

Un módulo de Apache es una extensión que añade una función al servidor web, como seguridad, acceso o compresión de páginas web.

### comandos ver modulos
```bash
apache2ctl -M
ls /etc/apache2/mods-available/
```
### comandos activan modulos

| Nº | Comando | Definición |
|---:|---|---|
| 1 | `sudo a2enmod rewrite` | Activa el módulo de reescritura de URL. |
| 2 | `sudo a2enmod headers` | Activa el módulo para modificar cabeceras HTTP. |
| 3 | `sudo systemctl restart apache2` | Reinicia Apache para aplicar los cambios. |
| 4 | `apache2ctl -M \| grep -E "rewrite\|headers\|ssl"` | Comprueba si los módulos rewrite, headers y ssl están cargados. |

![captura 1](fotos/Captura%2035.png)

mo0dulos tipicos 


| Módulo | Comandos | Descripción |
|---|---|---|
| **`mod_rewrite`** | `sudo a2enmod rewrite` | Permite reescribir y redirigir URL mediante reglas. |
| **`mod_headers`** | `sudo a2enmod headers` | Modifica las cabeceras HTTP para configurar seguridad y caché. |
| **`mod_ssl`** | `sudo a2enmod ssl` | Activa el soporte SSL/TLS para utilizar HTTPS. |
| **`mod_dir`** | `sudo a2enmod dir` | Busca automáticamente el archivo principal de una página, como `index.html`. |
| **`mod_alias`** | `sudo a2enmod alias` | Permite crear redirecciones y asociar URL con rutas del disco. |
| **`mod_auth_basic`** | `sudo a2enmod auth_basic` | Permite proteger páginas mediante usuario y contraseña. |
| **`mod_status`** | `sudo a2enmod status` | Permite consultar información sobre la actividad del servidor. |
| **`mod_php` / PHP-FPM** | Depende de la instalación | Permite a Apache procesar páginas web desarrolladas en PHP. |

- sudo a2enmod nombre_del_modulo (enciende el modulo)

- sudo a2dismod nombre_del_modulo (apaga el modulo)

(siempre despues de un cambio hay que poner el comando  sudo systemctl restart apache2 para restablecer el servidor con los cambios)

### pregunta ¿Qué ocurre si desactivas un módulo del que depende otro?

Apache fallará al intentar reiniciarse o la característica asociada dejará de funcionar de inmediato, mostrando errores en las páginas o bloqueando el servicio

# Apartado 8. Varios sitios en un mismo servidor (Virtual Hosts)

crearemos dos sitios en este servidor sitio.local y sitio2.local

### Crear los directorios y páginas

- sudo mkdir -p /var/www/sitio1 /var/www/sitio2

![captura 1](fotos/Captura%2041.png)

### Crear los ficheros de configuración
sudo nano /etc/apache2/sites-available/sitio1.conf

![captura 1](fotos/Captura%2036.png)


![captura 1](fotos/Captura%2037.png)

### Activar los sitios y desactivar el de por defecto

- sudo a2ensite sitio1.conf sitio2.conf (este comando sirve para activar los sitios web).

- sudo a2dissite 000-default.conf (este comando sirve para desactivar el sitio por defecto).

- sudo apache2ctl configtest (este comando sirve para comprobar que la configuración no tiene errores).

- sudo systemctl reload apache2 (este comando sirve para recargar la configuración sin cortar las conexiones).

- apache2ctl -S (este comando sirve para mostrar los sitios virtuales cargados).

### Resolución de nombres

![captura 1](fotos/Captura%2038.png)

![captura 1](fotos/Captura%2039.png)

### pregunta ¿Qué diferencia hay entre virtual hosts basados en nombre y basados en IP? ¿Cuál has usado?

Basados en nombre: Varios sitios web comparten la misma dirección IP y puerto, y Apache los diferencia leyendo el nombre del dominio y en IP: Cada sitio web utiliza una dirección IP distinta por lo que cada ip te lleva a una web concreta

he usado la dasados en nombre no basado en ip sitio1.local y sitio2.local

# Apartado 9. Permisos y propietarios

Apache se ejecuta con el usuario www-data. Comprueba:

- ps aux | grep apache2
- grep -E "APACHE_RUN_(USER|GROUP)" /etc/apache2/envvars
- ls -ld /var/www/sitio1 /var/www/sitio1/index.html

Aplica una política de permisos razonable:

- sudo chown -R www-data:www-data /var/www/sitio1
- sudo find /var/www/sitio1 -type d -exec chmod 755 {} \;
- sudo find /var/www/sitio1 -type f -exec chmod 644 {} \;

![captura 1](fotos/Captura%2040.png)


- Qué significan 755: Permisos que dan control total al propietario y lectura/ejecución al resto.

- Qué significan 644: Permisos que permiten leer y escribir al propietario y solo leer al resto.

- Por qué nunca se deben usar permisos 777: Porque permiten acceso total de lectura, escritura y ejecución a cualquiera, creando un riesgo crítico de seguridad.

- Por qué el contenido web no debe pertenecer a root ni ser escribible por todos: Para evitar que un fallo en la web comprometa

# Apartado 10 conclusion

En esta práctica he aprendido a instalar, configurar y administrar un servidor web Apache en Ubuntu 24.04. También he aprendido a comprobar su funcionamiento mediante comandos de terminal y a revisar los puertos, los servicios y los archivos de configuración.

He practicado la activación y desactivación de módulos, la modificación de la página de inicio, el cambio de puertos y la personalización de páginas de error. Además y a gestionar los permisos y propietarios de los archivos.

Durante la práctica he utilizado herramientas como `systemctl`, `apache2ctl`, `curl` y `a2enmod`, que permiten administrar y comprobar el servidor.

En conclusión, esta actividad me ha ayudado a comprender mejor cómo funciona un servidor web, cómo se alojan diferentes páginas y por qué es importante mantener una configuración correcta y aplicar medidas básicas de seguridad.








