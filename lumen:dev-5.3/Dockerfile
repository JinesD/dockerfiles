# Base image to use, this must be set as the first line
FROM php:7.0-apache

# maintainer: ran.ding <ran_ding@hotmail.com>
MAINTAINER ran.ding <ran_ding@hotmail.com>

LABEL version="1.0" \
description="this file create lumen image for develop evniroment by lumen5.3 & php7.0 with apache" \
time="2017.07.20"

# enviroment variables
# defined run user and some working directory
ENV APACHE_RUN_USER=www-data \
APACHE_RUN_GROUP=www-data \
APACHE_LOG_DIR=/var/log/apache2 \
APACHE_LOCK_DIR=/var/lock/apache2 \
APACHE_PID_FILE=/var/run/apache2.pid

# update apt-get cache and install some extensions for PHP
RUN apt-get update && apt-get install -y \
vim \
libfreetype6-dev \
libjpeg62-turbo-dev \
libmcrypt-dev \
libpng12-dev \
zlib1g-dev \
libssl-dev \
git \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) iconv mcrypt gd\
&& docker-php-ext-install gettext pdo pdo_mysql zip \
&& pecl install redis-3.1.3 mongodb-1.2.9 \
&& docker-php-ext-enable redis mongodb \
&& apt-get autoremove -y \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
&& apt-get clean \
&& a2enmod rewrite

# install composer(php package manager) & update package mirror
RUN curl -sS https://getcomposer.org/installer | php \
&& mv composer.phar /usr/local/bin/composer \
&& composer config -g repo.packagist composer https://packagist.phpcomposer.com

# install lumen
RUN composer global require "laravel/lumen-installer"

# set directory `/root/.composer/vendor/bin` to path
ENV PATH=/root/.composer/vendor/bin:$PATH

# copy config file at local enviroment to custom image
COPY conf/php.ini /usr/local/etc/php/php.ini
COPY conf/site.conf /etc/apache2/sites-enabled/site.conf

# exposed work port
EXPOSE 80
