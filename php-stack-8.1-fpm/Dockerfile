FROM php:8.1-fpm

LABEL maintainer="ashrul"
WORKDIR /var/www

### ARG SECTION ###
ARG POSTGRES_VERSION=12
ARG NODE_VERSION=lts
ARG WKHTML_VERSION=0.12.6.1-2
ARG WKHTML_HOST_VERSION=bullseye_amd64

### ENV SECTION ###
ENV TZ=Asia/Kuala_Lumpur
ENV PHPFPM__access.format '"%R - %u [%t] \"%m %r\" %s %l %Q %f"'

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && \
    apt-get install -y \
    # php php-fpm \
    wget \
    nano \
    gnupg2 \
    git \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    zip \
    systemctl \
    libfreetype6-dev \
    supervisor \
    gosu \
    nano

RUN docker-php-ext-install bcmath  

RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends

RUN apt-get update && \
    apt-get install -y \
    libmagickcore-dev \
    libmagickwand-dev

RUN apt-get clean

## locales
RUN apt-get install -y \
    locales \
    && locale-gen \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen

## PHP EXT mysql
RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-configure mysqli --with-mysqli=mysqlnd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli

# PHP EXT File exif
RUN docker-php-ext-install exif && docker-php-ext-enable exif

# Install the Microsoft ODBC driver for SQL Server
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN apt-get update  \
    && ACCEPT_EULA=Y apt-get install --yes --no-install-recommends msodbcsql17 unixodbc-dev \
    && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile \
    && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc 

# Install pdo sqlsrv
RUN pecl config-set php_ini /usr/local/etc/php/conf.d/php.ini \
    && pecl install sqlsrv \
    && pecl install pdo_sqlsrv \
    && echo 'extension=sqlsrv.so' >> /usr/local/etc/php/conf.d/sqlsrv.ini \
    && echo 'extension=pdo_sqlsrv.so' >> /usr/local/etc/php/conf.d/sqlsrv.ini 

# Install soap
RUN docker-php-ext-install soap

# PHP EXT Zip
RUN docker-php-ext-install zip

# PHP EXT GD
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# PHP EXT Redis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis 

# PHP EXT pcntl
RUN docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-install \
    pcntl

# Install MongoDB Client
RUN apt-get install -y libcurl4-openssl-dev pkg-config libssl-dev
RUN pecl uninstall mongodb
RUN pecl install mongodb && docker-php-ext-enable mongodb

# INSTALL IMAGICK
RUN pecl install imagick && docker-php-ext-enable imagick

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

## Clean repo cache
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN apt-get -y autoremove
RUN apt-get clean

# COPY ./ /var/www/statsdigital
# RUN mkdir /var/log/stacks

# #FOR YII2 INIT
# WORKDIR /var/wwwstatsdigital && php init

# for laravel only
# RUN chmod -R 775 storage bootstrap/cache
# RUN chown -R root:www-data storage bootstrap/cache
# RUN cp .env.production .env

# RUN composer install --prefer-dist --no-dev
# RUN composer install

# enable if your project have submodule or stacks is used as submodule
# RUN git submodule init && git submodule update

# for laravel only
# RUN composer require laravel/horizon league/flysystem-aws-s3-v3 "^3.0" && php artisan horizon:install
# RUN php artisan optimize \
# disable migrate on testing with docker compose if you are using local databases
# && php artisan migrate --force

# RUN cp -r /var/www/statsdigital/setup/nginx/nginx.conf /etc/nginx/conf.d/default.conf