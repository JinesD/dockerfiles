# Base image to use, this must be set as the first line
FROM php:7.1.17-apache

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
&& docker-php-ext-configure intl \
&& docker-php-ext-install -j$(nproc) iconv \
&& docker-php-ext-install mcrypt \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd \
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
COPY drivers/redis-3.1.5.tgz /tmp/redis-3.1.5.tgz
COPY drivers/memcached-3.0.4.tgz /tmp/memcached-3.0.4.tgz
COPY drivers/mongodb-1.3.4.tgz /tmp/mongodb-1.3.4.tgz
COPY drivers/swoole-2.0.10.tgz /tmp/swoole-2.0.10.tgz
RUN pear install /tmp/redis-3.1.5.tgz \
&& docker-php-ext-enable redis \
&& pear install /tmp/memcached-3.0.4.tgz \
&& docker-php-ext-enable memcached \
&& pear install /tmp/mongodb-1.3.4.tgz \
&& docker-php-ext-enable mongodb \
&& pear install /tmp/swoole-2.0.10.tgz \
&& docker-php-ext-enable swoole \
&& rm /tmp/redis-3.1.5.tgz /tmp/mongodb-1.3.4.tgz /tmp/swoole-2.0.10.tgz /tmp/memcached-3.0.4.tgz


# install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php \
&& php -r "unlink('composer-setup.php');" \
&& mv composer.phar /usr/local/bin/composer \
&& composer config -g repo.packagist composer https://packagist.phpcomposer.com

# copy config file at local enviroment to custom image
# COPY conf/php.ini /usr/local/etc/php/php.ini
# COPY conf/site.conf /etc/apache2/sites-enabled/site.conf

# exposed work port
EXPOSE 80
