

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
exit;
```
Sale de MariaDB y vuelve al terminal de Ubuntu.

### Captura del estado del servicio MariaDB

![Estado del servicio MariaDB](fotos/Captura%203.png)

### Captura de la entrada en MariaDB

![Entrada en MariaDB](fotos/Captura%204.png)












