

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


# Captura: Comprobación del estado de Apache

![Apache funcionando](fotos/Captura%201.png)
