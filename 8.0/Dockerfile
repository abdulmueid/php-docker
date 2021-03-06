ARG PHP_VERSION=8.0

FROM php:${PHP_VERSION}-fpm-alpine

ARG XDEBUG_VERSION=3.0.2
ARG APCU_VERSION=5.1.19
ARG IMAGICK_VERSION=3.4.4
ARG XMLRPC_VERSION=1.0.0RC2
ARG WITH_XDEBUG=false

RUN set -eux; \
    addgroup -g 1000 php && adduser -u 1000 -G php -D -H php; \
    apk --no-cache add --virtual .build-deps \
        $PHPIZE_DEPS \
        bzip2-dev \
        gettext-dev \
        libwebp-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libxpm-dev \
        freetype-dev \
        icu-dev \
        imagemagick-dev \
        libxml2-dev \
        libxslt-dev \
        libzip-dev \
        libgcrypt-dev \
        libgpg-error-dev \
    ; \
    docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
        --with-webp \
        --with-xpm \
    ; \
    docker-php-ext-configure zip; \
    docker-php-ext-install -j$(nproc) \
        bz2 \
        gettext \
        mysqli \
        bcmath \
        gd \
        exif \
        intl \
        opcache \
        pdo_mysql \
        soap \
        sockets \
        xsl \
        zip \
    ; \
    yes '' | pecl install apcu-${APCU_VERSION}; \
    docker-php-ext-enable apcu; \
#    yes '' | pecl install imagick-${IMAGICK_VERSION}; \
#    docker-php-ext-enable imagick; \
    yes '' | pecl install xmlrpc-${XMLRPC_VERSION}; \
    docker-php-ext-enable xmlrpc; \
    if [ $WITH_XDEBUG = "true" ] ; then \
        yes '' | pecl install xdebug-${XDEBUG_VERSION}; \
        docker-php-ext-enable xdebug; \
    fi ; \
    pecl clear-cache; \
    apk del .build-deps; \
    runDeps="$( \
    		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
    			| tr ',' '\n' \
    			| sort -u \
    			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    	)"; \
    apk add --no-cache --virtual .phpexts-rundeps $runDeps busybox-suid; \
    curl -sSL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
