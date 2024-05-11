# variable environnement
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
ARG VALUE_XDEBUG_VERSION
ENV DEF_XDEBUG_VERSION=${DEF_PHPXDEBUG_VERSION:-"${VALUE_XDEBUG_VERSION}"}
ENV XDEBUG_VERSION=${DEF_XDEBUG_VERSION:+"xdebug-${DEF_XDEBUG_VERSION}"}
ENV XDEBUG_VERSION=${XDEBUG_VERSION:-'xdebug'}

ENV DEF_PHPUNIT_VERSION=${VALUE_PHP_VERSION:-"11.1.3"}
ENV DEF_PHPUNIT_VERSION=${DEF_PHPUNIT_VERSION##${DEF_PHPX_VERSION}}
ARG VALUE_PHP_UNIT_VERSION
ENV DEF_PHP_UNIT_VERSION=${DEF_PHPUNIT_VERSION:-"${VALUE_PHP_UNIT_VERSION}"}
ENV PHP_UNIT_VERSION=${DEF_PHP_UNIT_VERSION:+"phpunit/phpunit:${DEF_PHP_UNIT_VERSION}"}
ENV PHP_UNIT_VERSION=${PHP_UNIT_VERSION:-'phpunit/phpunit'}

ARG VALUE_GOLANG_VERSION
ENV DEF_GOLANG_VERSION=${VALUE_GOLANG_VERSION:-"1.11.8"}
ARG GOLANG_VERSION="https://storage.googleapis.com/golang/go${DEF_GOLANG_VERSION}.linux-amd64.tar.gz"

ARG PHP_FOLDER_PROJECT
ENV PHP_FOLDER_PROJECT=${PHP_FOLDER_PROJECT:-"/usr/local/apache2/www"}
ENV PHP_FOLDER_LOG="/var/log/docker/php/"
ENV PHP_FOLDER_LOG_XDEBUG="/var/log/docker/xdebug/"
ENV PHP_FOLDER_TMP="/var/tmp/docker/php/"
ENV PHP_FOLDER_INIT="/var/docker/php/"
ENV PHP_FOLDER_INIT_DATA="/docker-entrypoint-initdata.d/"

# install package
RUN set -ex
RUN apt update
RUN apt install -y unzip
RUN apt install -y curl
RUN apt install -y git
RUN apt install -y cron
RUN apt install -y inotify-tools
#del list
RUN rm -rf /var/lib/apt/lists/*
   
#install mailhog
RUN curl -Lsf ${GOLANG_VERSION} | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail
    
# Goto temporary directory.
WORKDIR /tmp

# Install composer  
COPY --from=compvers /usr/bin/composer /usr/bin/composer  
RUN mkdir /var/composer  
ENV COMPOSER_HOME /var/composer  
ENV COMPOSER_ALLOW_SUPERUSER 1 

RUN mkdir -p ${PHP_FOLDER_PROJECT}
RUN mkdir -p ${PHP_FOLDER_INIT}
RUN mkdir -p ${PHP_FOLDER_LOG}
RUN mkdir -p ${PHP_FOLDER_LOG_XDEBUG}
RUN mkdir -p ${PHP_FOLDER_INIT}
RUN mkdir -p ${PHP_FOLDER_INIT_DATA}

COPY dockercron /etc/cron.d/dockercron
RUN crontab /etc/cron.d/dockercron

# xdebug.
RUN pecl install -o -f ${XDEBUG_VERSION} && rm -rf /tmp/pear
COPY xdebug_extends.sh ${PHP_FOLDER_INIT}
RUN chmod +x ${PHP_FOLDER_INIT}xdebug_extends.sh
RUN ${PHP_FOLDER_INIT}xdebug_extends.sh

COPY importdata.sh ${PHP_FOLDER_INIT}
RUN chmod +x ${PHP_FOLDER_INIT}importdata.sh

COPY docker-data-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-data-entrypoint.sh

RUN composer selfupdate && \
    composer require "${PHP_UNIT_VERSION}" --prefer-source --no-interaction && \
    ln -s /tmp/vendor/bin/phpunit /usr/local/bin/phpunit

ENTRYPOINT ["docker-data-entrypoint.sh", "docker-php-entrypoint"]

WORKDIR ${PHP_FOLDER_PROJECT}

CMD ["php-fpm"]
