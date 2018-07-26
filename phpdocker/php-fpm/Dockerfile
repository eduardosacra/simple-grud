FROM php:7.1-fpm

MAINTAINER Eduardo Sacramento de Sousa <eduardo.sacra.sousa@gmail.com>

# nginx and php-fpm user MUST be the same
RUN usermod -u 1000 www-data

RUN apt-get update && apt-get install -y \
    git \
    unzip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# install php extensions dependencies
RUN apt-get update && apt-get install -my mlocate wget \
 zlib1g-dev openssl libcurl4-openssl-dev libssl-dev \
 pkg-config libgdbm-dev libjpeg-dev libpng-dev libmcrypt-dev libicu-dev g++ \
 libxml2-dev

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Brazil/East /etc/localtime
RUN "date"

# Type docker-php-ext-install to see available extensions
# install php extensions
RUN docker-php-ext-install curl
RUN docker-php-ext-configure gd --with-jpeg-dir="/usr/lib/x86_64-linux-gnu"
RUN docker-php-ext-install gd
RUN docker-php-ext-install json
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install intl
RUN docker-php-ext-install opcache
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install pdo_mysql
RUN pecl install apcu && pecl install channel://pecl.php.net/apcu_bc-1.0.3

# install mongo
RUN pecl install mongodb
RUN echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/mongo.ini

# install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_host=dockerhost" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_log=/tmp/xdebug.log" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN { \
        echo 'apc.enabled=1'; \
        echo 'apc.shm_size=256M'; \
        echo 'apc.ttl=7200'; \
        echo 'apc.enable_cli=1'; \
        echo 'apc.gc_ttl=3600'; \
        echo 'apc.entries_hint=4096'; \
        echo 'apc.slam_defense=1'; \
        echo 'apc.serializer=php'; \
    } > /usr/local/etc/php/conf.d/docker-php-ext-apc.ini

ADD timezone.ini /usr/local/etc/php/conf.d/timezone.ini

RUN echo 'alias sf3="php bin/console"' >> ~/.bashrc

WORKDIR /aplication
