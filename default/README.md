# diphp docker image php <br />
Version 1.0.0

<details>
  <summary>Table des matières</summary>
  <ol>
    <li><a href="#Présentation">Présentation</a></li>
    <li>
        <a href="#Version">Version</a>
        <ul>
            <li><a href="#Modifier-les-versions">Modifier les versions</a></li>
            <li><a href="#Dernière-version">Dernière version</a></li>
        </ul>
    </li>
    <li>
        <a href="#Dossier">Dossier</a>
        <ul>
            <li><a href="#Projet">Projet</a></li>
            <li><a href="#Données">Donnés</a></li>
        </ul>
    </li>
    <li><a href="#Cron">Cron</a></li>
    <li><a href="#Docker-hub">Docker hub</a></li>
  </ol>
</details>

## Présentation

Pour créer une image php avec xdebug à partir du php de docker.

Les versions :
<ul>
  <li>php:8.3.7RC1</li>
  <li>Xdebug:3.3.2</li>
  <li>composer:2.7.4</li>
</ul>

> [!NOTE]
> Récupérer l'image du dossier « image ».

## Version

### Modifier les versions

Dans le fichier « docker-compose.yml » :

```
args:
    - VALUE_PHP_VERSION=8.3.7RC1-fpm
    - VALUE_XDEBUG_VERSION=3.3.2
    - VALUE_COMPOSER_VERSION=2.7.4
```

> [!NOTE]
> Prendre une version fpm pour php.

### Dernière version

Pour installer la dernière version à partir du fichier :

```
args:
    - VALUE_PHP_VERSION=fpm
    - VALUE_XDEBUG_VERSION=
    - VALUE_COMPOSER_VERSION=latest
```

## Dossier

### Projet

Par défaut le projet sera placé dans le dossier « /usr/local/apache2/www » dans le conteneur du php.
Vous pouvez le modifier dans un fichier  « docker-compose.yml » :

```
args:
    - PHP_FOLDER_PROJECT=/var/www
```

### Données

Mettre des données par défaut lors de la création du conteneur du php (images ou fichiers).

Créer un dossier « config/data » (par exemple) dans votre projet.

Dans un fichier « docker-compose.yml » :
```
volumes:
    - ./config/data:/docker-entrypoint-initdata.d:rw
```

## Cron

Pour mettre en place des tâches planifier, il suffit de créer un fichier « dockercron » dans votre projet.

Dans un fichier docker-compose.yml :
```
volumes:
    - ./dockercron:/etc/cron.d/dockercron:rw
```

Entrer le cron dans le fichier « dockercron », exemple :
```
*  *  *  *  * echo "Hello world!" >> /var/log/docker/php/test_cron.log
```


## Docker hub

Pour le transmettre sur docker hub, il serait préférable d'avoir une version fixe de l'image.

Remplacer :
```
ARG VALUE_PHP_VERSION
ARG DEF_PHP_VERSION=${VALUE_PHP_VERSION:-"8.3.7RC1-fpm"}
ARG VALUE_COMPOSER_VERSION
ARG DEF_COMPOSER_VERSION=${VALUE_COMPOSER_VERSION:-"2.7.4"}
# config install composer
FROM composer:${DEF_COMPOSER_VERSION} AS compvers

# config install php
FROM php:${DEF_PHP_VERSION}

# variable version (environnement)
ARG VALUE_PHP_VERSION
ENV DEF_PHPX_VERSION=${VALUE_PHP_VERSION}

ENV DEF_PHPXDEBUG_VERSION=${VALUE_PHP_VERSION:-"3.3.2"}
ENV DEF_PHPXDEBUG_VERSION=${DEF_PHPXDEBUG_VERSION##${DEF_PHPX_VERSION}}
ARG PHP_XDEBUG_VERSION
ENV DEF_XDEBUG_VERSION=${DEF_PHPXDEBUG_VERSION:-"${PHP_XDEBUG_VERSION}"}
ENV XDEBUG_VERSION=${DEF_XDEBUG_VERSION:+"xdebug-${DEF_XDEBUG_VERSION}"}
ENV XDEBUG_VERSION=${XDEBUG_VERSION:-'xdebug'}

ARG VALUE_GOLANG_VERSION
ENV DEF_GOLANG_VERSION=${VALUE_GOLANG_VERSION:-"1.11.8"}
ARG GOLANG_VERSION="https://storage.googleapis.com/golang/go${DEF_GOLANG_VERSION}.linux-amd64.tar.gz"
```

Par :
```
FROM composer:2.7.4 AS compvers

# config install php
FROM php:8.3.7RC1-fpm

# variable version (environnement)
ENV XDEBUG_VERSION="xdebug-3.3.2"
ARG GOLANG_VERSION="https://storage.googleapis.com/golang/go1.11.8.linux-amd64.tar.gz"
```
