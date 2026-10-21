

# Práctica: Instalación de MariaDB y PHP en Ubuntu 24.04

**Alumno:** Adrián Pérez Cereceda  
**Curso:** 2.º SMX  
**Módulo:** Aplicaciones Web  
**Fecha:** 21/10/2026

## Apartado 1. Preparación del sistema

```bash
sudo a2ensite 000-default.conf
```
Activa el sitio web predeterminado de Apache.

```bash
sudo systemctl reload apache2
```
Recarga la configuración de Apache para aplicar los cambios.

```bash
sudo apt update
```
Actualiza la lista de paquetes disponibles en los repositorios de Ubuntu.

```bash
sudo apt upgrade -y
```
Instala las actualizaciones disponibles del sistema. La opción `-y` acepta automáticamente las confirmaciones.

```bash
systemctl is-active apache2
```
Comprueba si Apache está funcionando. Debe mostrar `active`.


### Captura: Comprobación del estado de Apache

![Apache funcionando](fotos/Captura%201.png)


## Apartado 2. Instalación de MariaDB

```bash
sudo apt install mariadb-server -y
```
Instala MariaDB, el sistema que permite gestionar las bases de datos que utilizará WordPress.

```bash
mariadb --version
```
Muestra la versión de MariaDB instalada en el sistema.

### Captura de la versión de MariaDB

![Versión de MariaDB](fotos/Captura%202.png)


## Apartado 3. Comprobación del funcionamiento de MariaDB

```bash
sudo systemctl status mariadb
```
Comprueba el estado del servicio MariaDB. `active (running)` significa que está funcionando correctamente.

```bash
systemctl is-enabled mariadb
```
Comprueba si MariaDB se inicia automáticamente al encender el servidor. Debe aparecer `enabled`.

```bash
sudo mariadb
```
Permite entrar en MariaDB y comprobar que acepta conexiones.

```sql
exit
```
Sale de MariaDB y vuelve al terminal de Ubuntu.

### Captura del estado del servicio MariaDB

![Estado del servicio MariaDB](fotos/Captura%203.png)

### Captura de la entrada en MariaDB

![Entrada en MariaDB](fotos/Captura%204.png)


## Apartado 4. Comandos principales de MariaDB

| Nº | Comando | Función del comando |
|---|---|---|
| 1 | `sudo systemctl start mariadb` | Inicia el servicio MariaDB. |
| 2 | `sudo systemctl stop mariadb` | Detiene el servicio MariaDB. |
| 3 | `sudo systemctl restart mariadb` | Reinicia el servicio MariaDB. |
| 4 | `sudo systemctl status mariadb` | Muestra el estado del servicio MariaDB. |
| 5 | `sudo systemctl enable mariadb` | Configura MariaDB para que se inicie automáticamente al encender el servidor. |



## Apartado 5. ¿Qué pasa si MariaDB está parado?

```bash
sudo systemctl stop mariadb
```
Detiene temporalmente el servicio MariaDB para comprobar qué ocurre cuando está apagado.

```bash
systemctl is-active mariadb
```
Comprueba si MariaDB está funcionando. Debe aparecer `inactive` cuando el servicio está detenido.

```bash
sudo mariadb
```
Intenta entrar en MariaDB. Como el servicio está parado, aparecerá un error de conexión.

### Captura del error al intentar entrar en MariaDB

![Error de conexión con MariaDB](fotos/Captura%205.png)

```bash
sudo systemctl start mariadb
```
Vuelve a iniciar el servicio MariaDB para recuperar su funcionamiento.

```bash
systemctl is-active mariadb
```
Comprueba que MariaDB vuelve a funcionar. Debe aparecer `active`.

### Captura de la recuperación de MariaDB

![Recuperación de MariaDB](fotos/Captura%206.png)

### Pregunta de reflexión

**Si WordPress no pudiera conectar con la base de datos, ¿qué dos comandos usarías para saber si MariaDB está parado?**

Utilizaría `systemctl is-active mariadb` para comprobar si el servicio está activo y `sudo systemctl status mariadb` para consultar su estado detallado.


## Apartado 6. Instalación de PHP

```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl -y
```
Instala PHP y los módulos necesarios para que WordPress funcione con Apache y MariaDB.

```bash
php -v
```
Muestra la versión de PHP instalada. En este caso, se ha instalado PHP 8.3.6.

### Captura de la versión de PHP

![Versión de PHP](fotos/Captura%207.png)

```bash
sudo systemctl restart apache2
```
Reinicia Apache para que cargue correctamente la configuración de PHP.

```bash
sudo apache2ctl -M | grep -E "mpm|php"
```
Comprueba que Apache tiene cargados los módulos necesarios para ejecutar PHP. Deben aparecer `mpm_prefork_module` y `php_module`.

```bash
php -m | grep -i mysql
```
Comprueba que están instalados los módulos que permiten a PHP conectarse con MariaDB: `mysqli`, `mysqlnd` y `pdo_mysql`.

### Captura de los módulos de Apache y PHP

![Módulos de Apache y PHP](fotos/Captura%208.png)

```bash
echo '<?php phpinfo();' | sudo tee /var/www/html/info.php
```
Crea un archivo de prueba que muestra información sobre PHP al abrirlo desde el navegador.

### Captura de la página de información de PHP

![Página de información de PHP](fotos/Captura%209.png)

```bash
sudo rm /var/www/html/info.php
```
Elimina el archivo de prueba para evitar exponer información técnica del servidor.

```bash
ls /var/www/html
```
Muestra los archivos que quedan en el directorio web después de eliminar `info.php`.

### Pregunta:

**¿Por qué es peligroso dejar `info.php` en un servidor real? ¿Para qué sirve el paquete `php-mysql`?**

Dejar `info.php` puede revelar información técnica del servidor que podría ayudar la gente obtenga datos sobre tu servidor. El paquete `php-mysql` permite que PHP se conecte con MariaDB para consultar y guardar los datos de WordPress.








