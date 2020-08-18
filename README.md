# php-docker
Docker images for PHP with extras, based on official PHP7 images on Docker hub.

All images include the following extensions:
  - apcu
  - bz2
  - gettext
  - mysqli
  - bcmath
  - gd
  - intl
  - opcache
  - pdo_mysql
  - soap
  - sockets
  - xmlrpc
  - xsl
  - zip

Config file mount points:
  - /usr/local/etc/php/php.ini
  - /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
  
Available via Docker hub:
  - docker pull abdulmueid/php:7.4-fpm
  - docker pull abdulmueid/php:7.4-fpm-xdebug
  - docker pull abdulmueid/php:7.3-fpm
  - docker pull abdulmueid/php:7.3-fpm-xdebug
  - docker pull abdulmueid/php:7.2-fpm
  - docker pull abdulmueid/php:7.2-fpm-xdebug
