# Base image to use, this must be set as the first line
FROM php:5.6.40-apache

# enviroment variables
# defined run user and some working directory
ENV APACHE_RUN_USER=www-data \
APACHE_RUN_GROUP=www-data \
APACHE_LOG_DIR=/var/log/apache2 \
APACHE_LOCK_DIR=/var/lock/apache2 \
APACHE_PID_FILE=/var/run/apache2.pid

# update apt-get cache and install some extensions for PHP
RUN apt-get update && apt-get install -y --no-install-recommends \
libfreetype6-dev \
libjpeg62-turbo-dev \
libmemcached-dev \
libmagickwand-dev \
libmcrypt-dev \
mcrypt \
libpng-dev \
zlib1g-dev \
libssl-dev \
libpq-dev \
libicu-dev \
libxml2-dev \
wget \
g++ \
libldap2-dev \
zlib1g-dev \
libgearman-dev \
libgearman-client-perl \
openssl \
&& docker-php-ext-configure intl \
&& docker-php-ext-install -j$(nproc) iconv \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) mcrypt gd \
&& docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
&& docker-php-ext-install ldap \
&& docker-php-ext-install gettext \
&& docker-php-ext-install pdo pdo_mysql \
&& docker-php-ext-install zip \
&& docker-php-ext-install soap \
&& docker-php-ext-install opcache \
&& apt-get autoremove -y \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
&& apt-get clean \
&& a2enmod rewrite

# install extensions
COPY extras/redis-4.2.0.tgz /tmp/redis-4.2.0.tgz
COPY extras/gearman-1.1.2.tgz /tmp/gearman-1.1.2.tgz
RUN pecl install /tmp/redis-4.2.0.tgz /tmp/gearman-1.1.2.tgz \
&& docker-php-ext-enable redis gearman \
&& rm /tmp/redis-4.2.0.tgz /tmp/gearman-1.1.2.tgz

# exposed work port
EXPOSE 80
