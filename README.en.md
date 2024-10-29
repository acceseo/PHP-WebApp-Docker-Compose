<div align="center">
    <a href="https://www.acceseo.com">
        <img alt="acceseo logo" src="logo.svg" width="150">
    </a>
</div>

<h1 align="center">PHP WebApp Docker Compose</h1>
<div align="center">
    <a href="README.md">Spanish version</a>
    <br><br>
</div>

<hr>

**Index**
- [📖 Description](#-description)
- [📚 Prerequisites](#-prerequisites)
- [🔨 Getting started](#-getting-started)
  - [🟢 Available services](#-available-services)
- [🤖 Useful commands](#-useful-commands)
  - [PHP](#php)
  - [Node](#node)
  - [MariaDB](#mariadb)
- [🪄 Advanced config](#-advanced-config)
  - [Disable Xdebug](#how-to-disable-xdebug)
  - [Symfony usage example](#-symfony-usage-example)
- [:question: FAQs](#question-faqs)
- [📄 License](#-license)
- [👷 Credits](#-credits)

## 📖 Description
Basic configuration file for creating web applications (Symfony, WordPress, PrestaShop).

## 📚 Prerequisites
- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## 🔨 Getting started
1. Download the config file `docker-compose.yml` in your project folder
```shell
curl -O https://raw.githubusercontent.com/acceseo/PHP-WebApp-Docker-Compose/main/docker-compose.yml
```
2. Start the containers
```shell
docker compose up -d
```
#### 🟢 Available services
* Web server
  * http://localhost:80
  * http://localhost:443
* Database
  * localhost:3306
* Mail server
  * http://localhost:8025 (webmail interface)
  * localhost:1025 (SMTP server)
---

## 🤖 Useful commands
### PHP
* **Install dependencies with composer**
```shell
docker compose exec php.local composer require symfony/finder
```
* **Run a script**
```shell
docker compose exec php.local script.php
```
* **Run wp-cli**
```shell
docker compose exec php.local wp
```

### Node
* **Run webpack**
```shell
docker compose run --rm node.local node webpack
```
* **Install dependencies**
```shell
docker compose run --rm node.local npm install
```

### MariaDB
* **Import a database dump from the terminal**
```shell
docker compose exec -T mariadb.local mysql -uroot -proot _database_ < dump.sql
```
* **Generate a database dump from the terminal**
```shell
docker compose exec -T mariadb.local mysqldump -uroot -proot app > dump.sql
```
* **Create a database**
```shell
docker compose exec -T mariadb.local mysql -uroot -proot -e "CREATE DATABASE db"
```
---

## 🪄 Advanced config
### How to disable Xdebug
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

## 🚀 Symfony usage example
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
- **¿Why do we use `profiles` for the Node container?**
To avoid creating the Node container each time you run `docker compose up`.

## 📄 License
This project is under the [MIT License](LICENSE).

## 👷 Credits
* Created by [acceseo](https://www.acceseo.com).
