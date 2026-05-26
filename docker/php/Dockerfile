FROM php:8.2.8-fpm-alpine

# Copy File Config
ADD ./docker/php/www.conf /usr/local/etc/php-fpm.d/www.conf
ADD ./docker/php/memory-limit-php.ini /usr/local/etc/php/conf.d/memory-limit-php.ini
ADD ./docker/php/max-execution-time-php.ini /usr/local/etc/php/conf.d/max-execution-time-php.ini

# ADD and set Group
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

# Create folder to run
RUN mkdir -p /var/www/html

# Set Profile
RUN chown laravel:laravel /var/www/html

# Work in the specific space
WORKDIR /var/www/html

# Install dependencies
RUN apk add --no-cache \
    freetype \
    libpng \
    libjpeg-turbo \
    freetype-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    unixodbc-dev

RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg

RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} gd

RUN apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN docker-php-ext-install pdo pdo_mysql

RUN docker-php-ext-configure pcntl --enable-pcntl \
  && docker-php-ext-install \
    pcntl

# install and enable xdebug
RUN apk add --no-cache $PHPIZE_DEPS

# Install MS ODBC driver 17
#RUN curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/msodbcsql18_18.2.1.1-1_amd64.apk
#RUN curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/mssql-tools18_18.2.1.1-1_amd64.apk
#RUN apk add --allow-untrusted msodbcsql18_18.2.1.1-1_amd64.apk
#RUN apk add --allow-untrusted mssql-tools18_18.2.1.1-1_amd64.apk

# Install MS SQL PHP Drivers
#RUN pecl install sqlsrv
#RUN pecl install pdo_sqlsrv
#RUN docker-php-ext-enable sqlsrv
#RUN docker-php-ext-enable pdo_sqlsrv
