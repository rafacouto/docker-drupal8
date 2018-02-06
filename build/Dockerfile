
FROM php:7.1-fpm

ENV APP_HOME=/app/drupal8 \
    APP_USER=drupal8

COPY drupal8.ini /usr/local/etc/php/conf.d/

RUN set -ex \
    && buildDeps=' \
        libbz2-dev \
        libcurl4-openssl-dev \
        libfreetype6-dev \
        libicu-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libmemcached-dev \
        libpng12-dev \
        libpq-dev \
        libssl-dev \
        libxml2-dev \
        zlib1g-dev \
        ' \
    && apt-get update \
    && apt-get install -y --no-install-recommends $buildDeps \
        libicu52 \
        mysql-client \
    && docker-php-ext-configure gd \
        --with-jpeg-dir=/usr/include \
        --with-png-dir=/usr/include \
        --with-freetype-dir=/usr/include \
    && docker-php-ext-install -j "$(nproc)" \
        bcmath \
        bz2 \
        calendar \
        curl \
        exif \
        dom \
        gd \
        gettext \
        iconv \
        intl \
        mbstring \
        mcrypt \
        opcache \
        soap \
        sockets \
        pdo \
        pdo_mysql \
        pdo_pgsql \
        zip \
    && apt-mark manual \
        libfreetype6 \
        libjpeg62-turbo \
        libmcrypt4 \
        libpq5 \
    && apt-get purge -y --auto-remove $buildDeps \
    && rm -rf /var/lib/apt/lists/* 

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN curl -o /usr/local/bin/composer https://getcomposer.org/composer.phar \
    && chmod +x /usr/local/bin/composer 

WORKDIR $APP_HOME

RUN useradd -s /bin/bash -d $APP_HOME -M $APP_USER \
    && chown -R $APP_USER $APP_HOME \
    && echo 'export PATH=$PATH:$APP_HOME/web/vendor/bin' >> /etc/profile

USER $APP_USER

RUN composer create-project --no-dev --prefer-dist drupal/drupal web

