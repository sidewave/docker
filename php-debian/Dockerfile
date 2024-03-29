FROM debian:11-slim

LABEL authors="Michael Pavlista <pavlista@sidewave.io>"

ARG PHP_VERSION
ARG TARGETPLATFORM

ENV DEBIAN_FRONTEND=noninteractive \
    \
    APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2 \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    APACHE_PID_FILE=/var/run/apache2.pid \
    APACHE_DOCUMENT_ROOT=/var/www/html/ \
    \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PHP_VERSION=${PHP_VERSION}

RUN \
    bash -c '[[ $PHP_VERSION =~ ^(8\.[01])$ ]] || { echo "Invalid build argument PHP_VERSION (available: 8.0, 8.1)"; exit 1; }' && \
    \
    sh -c 'printf "Package: *\nPin: release o=deb.sury.org,n=buster\nPin-Priority: 900\n" > /etc/apt/preferences.d/sury.pref' && \
    \
    apt-get update && \
    apt-get install -y --no-install-recommends dialog apt-utils git mc htop nmap wget curl unzip ssh cron \
    apt-transport-https ca-certificates gnupg2 lsb-release openssh-client && \
    \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list' && \
    apt-get update && \
    \
    apt-get install -y --no-install-recommends apache2 && \
    a2enmod include && \
    a2enmod rewrite && \
    a2enmod proxy_fcgi && \
    a2enmod expires && \
    a2enmod filter && \
    a2enmod headers && \
    a2enmod ssl && \
    rm /etc/apache2/sites-enabled/000-default.conf && \
    echo "umask 000" >>  /etc/apache2/envvars && \
    \
    apt-get update && \
    apt-get install -y --no-install-recommends php${PHP_VERSION} php${PHP_VERSION}-cli libapache2-mod-php${PHP_VERSION} \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-pdo \
    php${PHP_VERSION}-mysql \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-imagick \
    php${PHP_VERSION}-redis \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-imap \
    php${PHP_VERSION}-soap \
    php${PHP_VERSION}-xdebug && \
    bash -c 'ln -s /usr/bin/php${PHP_VERSION} /usr/bin/php${PHP_VERSION//./}' && \
    \
    chown www-data:www-data /var/lib/php/sessions && \
    \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    ln -s /usr/local/bin/composer /usr/bin/composer && \
    \
    mkdir /var/log/docker/ && chmod 777 /var/log/docker/ && \
    \
    apt-get remove -y python3.7 && \
    apt-get clean -y && apt-get autoclean -y && apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/* && \
    \
    rm /etc/init.d/apache2

COPY ./files/apache/apache2.conf /etc/apache2/
COPY ./files/apache/default-sites.conf /etc/apache2/sites-enabled/

COPY ./files/php/php.ini /etc/php/${PHP_VERSION}/apache2/
COPY ./files/php/xdebug.ini /etc/php/${PHP_VERSION}/apache2/conf.d/
COPY ./files/php/php.ini /etc/php/${PHP_VERSION}/cli/
COPY ./files/php/xdebug.ini /etc/php/${PHP_VERSION}/cli/conf.d/

WORKDIR /var/www/html
