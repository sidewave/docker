FROM debian:10-slim

LABEL authors="Michael Pavlista <pavlista@sidewave.io>"

ENV DEBIAN_FRONTEND=noninteractive \
    \
    APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2 \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    APACHE_PID_FILE=/var/run/apache2.pid \
    \
    COMPOSER_ALLOW_SUPERUSER=1

RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends dialog apt-utils git mc htop nmap wget curl unzip \
    apt-transport-https ca-certificates gnupg2 lsb-release openssh-client && \
    \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list' && \
    apt-get update && \
    \
    apt-get install -y --no-install-recommends apache2 && \
    a2enmod include && \
    a2enmod rewrite && \
    a2enmod expires && \
    a2enmod headers && \
    a2enmod ssl && \
    \
    rm /etc/apache2/sites-enabled/000-default.conf && \
    \
    apt-get update && \
    apt-get install -y --no-install-recommends php7.4 php7.4-cli libapache2-mod-php7.4 \
    php7.4-curl \
    php7.4-intl \
    php7.4-gd \
    php7.4-mbstring \
    php7.4-pdo \
    php7.4-mysql \
    php7.4-json \
    php7.4-zip \
    php7.4-xml \
    php7.4-imagick \
    php7.4-redis \
    php7.4-bcmath \
    php7.4-imap && \
    \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    ln -s /usr/local/bin/composer /usr/bin/composer && \
    composer global require "hirak/prestissimo:^0.3" && \
    composer clear-cache && \
    \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs=12.* && \
    npm install -g webpack@~4 webpack-cli && \
    npm cache clean --force && \
    \
    apt-get remove -y python3.7 && \
    apt-get clean -y && apt-get autoclean -y && apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/* && \
    \
    rm /etc/init.d/apache2

COPY ./files/apache/apache2.conf /etc/apache2/
COPY ./files/php/php.ini /etc/php/7.4/apache2/
COPY ./files/php/php.ini /etc/php/7.4/cli/

CMD apache2ctl -D FOREGROUND

WORKDIR /var/www/html
