
FROM php:7-fpm

ENV APP_HOME=/app/drupal8 \
    WWW_ROOT=$APP_HOME/web

WORKDIR $APP_HOME

COPY drupal8.ini /usr/local/etc/php/conf.d/

RUN set -ex \
	&& buildDeps=' \
		libfreetype6-dev \
        	libjpeg62-turbo-dev \
        	libpng12-dev \
        	libmcrypt-dev \
	        libssl-dev \
        	libpq-dev \
	        libbz2-dev \
        	libmemcached-dev \
	        libcurl4-openssl-dev \
        	libxml2-dev \
		zlib1g-dev \
		' \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends $buildDeps \
	&& docker-php-ext-configure gd \
		--with-jpeg-dir=/usr/include \
		--with-png-dir=/usr/include \
		--with-freetype-dir=/usr/include \
	&& docker-php-ext-install -j "$(nproc)" \
		gd \
		mbstring \
		opcache \
		pdo \
		pdo_mysql \
		pdo_pgsql \
		zip \
		iconv \
		mcrypt \
		bcmath \
		bz2 \
		calendar \
		exif \
		dom \
		gettext \
		opcache \
		soap \
		sockets \
		curl \
	&& apt-mark manual \
		libmcrypt4 \
		libjpeg62-turbo \
		libpq5 \
		libfreetype6 \
	&& apt-get purge -y --auto-remove $buildDeps \
	&& rm -rf /var/lib/apt/lists/* 

RUN curl -o /usr/local/bin/composer https://getcomposer.org/composer.phar \
	&& chmod +x /usr/local/bin/composer \
	&& composer self-update

RUN echo 'export PATH=$PATH:$APP_HOME/vendor/bin' >> /etc/profile

