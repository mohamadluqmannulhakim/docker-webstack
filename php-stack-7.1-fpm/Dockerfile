FROM php:7.1-fpm

# RUN apt-get update && apt-get install -y 
RUN apt-get update && apt-get install -y  \
    freetds-bin \
    freetds-dev \
    freetds-common \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    gnupg2 \
    libxml2-dev \
    && apt-get install -y locales \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && locale-gen 

RUN docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-configure mysqli --with-mysqli=mysqlnd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install bcmath  

# Install soap
RUN docker-php-ext-install soap

# PHP EXT File exif
RUN docker-php-ext-install exif && docker-php-ext-enable exif

# PHP EXT Zip
RUN docker-php-ext-install zip

# PHP EXT GD
# RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-configure gd --with-freetype-dir --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

# PHP EXT Redis
# # RUN apt-get install -y php-redis
# RUN pecl install -o -f redis \
#     &&  rm -rf /tmp/pear \
#     &&  docker-php-ext-enable redis \
#     &&  docker-php-ext-enable redis.so 
# # RUN apk add --no-cache pcre-dev $PHPIZE_DEPS \
# #     && pecl install redis \
# #     && docker-php-ext-enable redis.so
# RUN apt-get install php-redis

# RUN echo "extension=redis.so" >> /usr/local/etc/php/conf.d/php.ini
# RUN pecl install --force redis \
#     && rm -rf /tmp/pear \
#     && docker-php-ext-enable redis \
#     && docker-php-ext-enable redis.so

# PHP EXT pcntl
# RUN docker-php-ext-configure pcntl --enable-pcntl \
#     && docker-php-ext-install \
#     pcntl

# PHP EXT intl
# RUN apt-get -y update \
#     && apt-get install -y zlib1g-dev libicu-dev g++\ 
#     && docker-php-ext-configure intl --enable-intl \
#     && docker-php-ext-install \
#     intl

# remove list
RUN rm -rf /var/lib/apt/lists/*

# install xdebug
# RUN pecl install xdebug
# RUN apt-get update
# RUN apt-get install -y php-xdebug
RUN apt-get -y install gcc make autoconf libc-dev pkg-config
# RUN pecl install xdebug
# RUN apt-get install php-xdebug
# RUN docker-php-ext-enable xdebug

RUN pecl channel-update pecl.php.net
RUN pecl install xdebug-2.7.2 \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9596" >> /usr/local/etc/php/conf.d/xdebug.ini

# RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.remote_autostart=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.default_enable=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.profiler_enable=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
# RUN echo "xdebug.remote_log=\"/tmp/xdebug.log\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini




ENV LOCALTIME Asia/Kuala_Lumpur
ENV PHPFPM__access.format '"%R - %u [%t] \"%m %r\" %s %l %Q %f"'

WORKDIR /var/www

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


### Special modifications for the www-data user.

RUN groupadd -og 999 docker && usermod -a -G docker www-data


# Add www-data environment (for SSH mostly)
#RUN sed -i 's/\#umask 022/umask 002/' /etc/skel/.profile
#RUN usermod -d /home/www-data -s /bin/bash www-data \
#    && cp -av /etc/skel /home/www-data \
#    && mv /var/www /home/www-data/www \
#    && ln -s /home/www-data/www /var/www \
#    && chown -R www-data:www-data /home/www-data


