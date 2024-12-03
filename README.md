<div align="center">
    <a href="https://www.acceseo.com">
        <img alt="acceseo logo" src="logo.svg" width="150">
    </a>
</div>

<h1 align="center">PHP WebApp Docker Compose</h1>
<div align="center">
    <a href="README.en.md">English version</a>
    <br><br>
</div>

<hr>

**Índice**
- [📖 Descripción](#-descripción)
- [📚 Requisitos previos](#-requisitos-previos)
- [🔨 Primeros Pasos](#-primeros-pasos)
  - [🟢 Servicios disponibles](#-servicios-disponibles)
- [🤖 Comandos Útiles](#-comandos-útiles)
  - [PHP](#php)
  - [Node](#node)
  - [MariaDB](#mariadb)
- [🪄 Configuraciones Avanzadas](#-configuraciones-avanzadas)
  - [Configurar Xdebug en Visual Studio Code](#configurar-xdebug-en-visual-studio-code)
  - [Cómo Desactivar Xdebug](#cómo-desactivar-xdebug)
  - [Ejemplo de uso Symfony](#-ejemplo-de-uso-symfony)
- [:question: FAQs](#question-faqs)
- [📄 Licencia](#-licencia)
- [👷 Créditos](#-créditos)

## 📖 Descripción
Archivo de configuración base para crear aplicaciones web (Symfony, WordPress, PrestaShop).

## 📚 Requisitos Previos
- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## 🔨 Primeros pasos
1. Descarga el archivo de configuración `docker-compose.yml` en la carpeta de tu proyecto
```shell
curl -O https://raw.githubusercontent.com/acceseo/PHP-WebApp-Docker-Compose/main/docker-compose.yml
```
2. Inicia los contenedores
```shell
docker compose up -d
```
#### 🟢 Servicios disponibles
* Servidor web
  * http://localhost (:80)
  * https://localhost (:443)
* Base de datos
  * localhost:3306
* Servidor de correo
  * http://localhost:8025 (interfaz webmail)
  * localhost:1025 (servidor SMTP)
---

## 🤖 Comandos útiles
### PHP
* **Instalación de dependencias con composer**
```shell
docker compose exec php.local composer require symfony/finder
```
* **Ejecución de un script**
```shell
docker compose exec php.local script.php
```
* **Ejecución de wp-cli**
```shell
docker compose exec php.local wp
```

### Node
* **Ejecución de webpack**
```shell
docker compose run --rm node.local node webpack
```
* **Instalación de dependencias**
```shell
docker compose run --rm node.local npm install
```

### MariaDB
* **Importar dump de la base de datos desde el terminal**
```shell
docker compose exec -T mariadb.local mysql -uroot -proot _database_ < dump.sql
```
* **Generar dump de la base de datos desde el terminal**
```shell
docker compose exec -T mariadb.local mysqldump -uroot -proot app > dump.sql
```
* **Crear una base de datos**
```shell
docker compose exec -T mariadb.local mysql -uroot -proot -e "CREATE DATABASE db"
```
---

## 🪄 Configuraciones Avanzadas
### Configurar Xdebug en Visual Studio Code
Ejecuta los pasos indicados en [acceseo/xdebug-config-for-vscode](https://github.com/acceseo/xdebug-config-for-vscode)
### Cómo desactivar Xdebug
* **CLI**
```shell
docker compose exec -e XDEBUG_MODE=off php.local my-script.php
```
* **Web**
```yaml
  php.local:
    image: acceseo/php-fpm:8.3
    volumes:
      - .:/app
    environment:
      - XDEBUG_MODE=off
```
---

## 🚀 Ejemplo de uso: Symfony
```yaml
...
httpd.local:
    image: acceseo/httpd
    environment:
      - HTTPD_APP_DIRECTORY=/app/public
...
```
---

## :question: FAQs
- **¿Por qué usamos `profiles` para el contenedor de Node?**
Para evitar que el contenedor de Node se cree cada vez que ejecutas `docker compose up`.

## 📄 Licencia
Este proyecto está bajo la [Licencia MIT](LICENSE).

## 👷 Créditos
* Creado por [acceseo](https://www.acceseo.com).
