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
  </ol>
</details>

## Présentation

Pour créer un image php avec xdebug à partie du php de docker.

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
Il suffit de placer les fichiers dans le dossier « /docker-entrypoint-initdata.d » du conteneur php.

## Cron

Pour mettre en place des tâches planifier, il suffit de modifier le fichier « /etc/cron.d/dockercron », enregister le contenu.
Lancer dans le terminal du conteneur php :

```
$ crontab /etc/cron.d/dockercron
```
