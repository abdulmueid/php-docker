ARG PHP_VERSION=7.3

FROM php:${PHP_VERSION}-fpm-alpine

ARG XDEBUG_VERSION=2.9.2
ARG APCU_VERSION=5.1.18
ARG WITH_XDEBUG=false

RUN set -eux; \
    apk --no-cache add --virtual .build-deps \
        $PHPIZE_DEPS \
        libwebp-dev \
        libjpeg-turbo-dev \
        libpng-dev libxpm-dev \
        freetype-dev \
        icu-dev \
        libxml2-dev \
        libxslt-dev \
        libzip-dev \
    ; \
    docker-php-ext-configure gd \
        --with-freetype-dir \
        --with-jpeg-dir \
        --with-webp-dir \
        --with-png-dir \
        --with-zlib-dir \
        --with-xpm-dir \
    ; \
    docker-php-ext-configure zip --with-libzip; \
    docker-php-ext-install -j$(nproc) \
        mysqli \
        bcmath \
        gd \
        intl \
        opcache \
        pdo_mysql \
        soap \
        sockets \
        xmlrpc \
        xsl \
        zip \
    ; \
    yes '' | pecl install apcu-${APCU_VERSION}; \
    docker-php-ext-enable apcu; \
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
    apk add --no-cache --virtual .phpexts-rundeps $runDeps; \
    curl -sSL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
