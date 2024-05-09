# diphp docker image php <br />
Version 1.0.0

<details>
  <summary>Table des matières</summary>
  <ol>
    <li><a href="#Présentation">Présentation</a></li>
    <li>
        <a href="#Installation">Installation</a>
        <ul>
            <li><a href="#Le-nom-du-projet">Le nom du projet</a></li>
            <li><a href="#Le-fichier-env">Le fichier .env</a></li>
            <li><a href="#Création-des-conteneurs">Création des conteneurs</a></li>
            <li><a href="#Où-placer-le-code">Où placer le code</a></li>
        </ul>
    </li>
    <li>
        <a href="#Autres-informations">Autres informations</a>
        <ul>
            <li><a href="#Versions">Versions</a></li>
            <li><a href="#Installer-la-dernière-version">Installer la dernière version</a></li>
            <li><a href="#Config">Config</a></li>
            <ul>
                <li><a href="#Configurations-du-SGBD-du-site">Configurations SGBD du site</a></li>
                <li><a href="#php.ini-et-httpd.conf">php.ini et httpd.conf</a></li>
                <li><a href="#Xdebug">Xdebug</a></li>
            </ul>
            <li><a href="#Stockages">Stockages</a></li>
            <ul>
                <li><a href="#Les-données-de-la-base-de-données">Les données de la base de données</a></li>
                <li><a href="#Les-données-de-mailhog">Les données de mailhog</a></li>
            </ul>
            <li><a href="#Dossier-config-dans-www">Dossier config dans www</a></li>
        </ul>
    </li>
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

### Modifier les version

Dans un fichier « docker-compose.yml » :
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
