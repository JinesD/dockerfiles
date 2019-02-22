FROM php:7.2.15-apache

ENV APACHE_RUN_USER=www-data \
APACHE_RUN_GROUP=www-data \
APACHE_LOG_DIR=/var/log/apache2 \
APACHE_LOCK_DIR=/var/lock/apache2 \
APACHE_PID_FILE=/var/run/apache2.pid

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
git

RUN cd /home \
&& git clone https://github.com/wcgallego/pecl-gearman.git \
&& cd pecl-gearman \
&& phpize \
&& ./configure \
&& make \
&& make install \
&& echo 'extension=gearman.so' > /usr/local/etc/php/conf.d/docker-php-ext-gearman.ini \
&& cd .. \
&& rm -rf pecl-gearman

RUN docker-php-ext-configure intl \
&& docker-php-ext-install -j$(nproc) iconv \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd \
&& docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
&& docker-php-ext-install ldap \
&& docker-php-ext-install gettext \
&& docker-php-ext-install pdo pdo_mysql \
&& docker-php-ext-install zip \
&& docker-php-ext-install soap \
&& docker-php-ext-install opcache \
&& docker-php-ext-install bcmath \
&& apt-get autoremove -y \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
&& apt-get clean \
&& a2enmod rewrite

COPY extras/redis-4.2.0.tgz /tmp/redis-4.2.0.tgz
RUN pecl install /tmp/redis-4.2.0.tgz \
&& docker-php-ext-enable redis  \
&& rm /tmp/redis-4.2.0.tgz

EXPOSE 80