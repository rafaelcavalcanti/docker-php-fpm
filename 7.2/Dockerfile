FROM php:7.2.1-fpm-alpine
MAINTAINER Tony C. B. <tonycbatista@gmail.com>

ENV TZ America/Sao_Paulo
ENV PHP_MEMORY_LIMIT 128M
ENV XDEBUG_VERSION 2.6.0

RUN echo 'deb http://httpredir.debian.org/debian jessie contrib' >> /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -y
RUN export DEBIAN_FRONTEND=noninteractive && apt-get install -y libssl-dev curl \
    libcurl4-gnutls-dev \
    libxml2-dev \
    libicu-dev \
    libpq-dev \
    libbz2-dev \
    libmcrypt4 \
    libmemcached11 \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng16-16 \
    libmcrypt-dev \
    libpng-dev \
    openssl \
    make \
    gcc \
    re2c \
    g++ \
    autoconf \
    bash \
    git \
    curl \
    wget

# timezone
RUN export DEBIAN_FRONTEND=noninteractive && apt-get install -y tzdata && \
    cp /usr/share/zoneinfo/${TZ} /etc/localtime && \
    echo "${TZ}" > /etc/timezone

# extra php modules
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install bz2
RUN docker-php-ext-install ctype
RUN docker-php-ext-install curl
RUN docker-php-ext-install iconv
RUN docker-php-ext-install intl
RUN docker-php-ext-install json
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install opcache
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install phar
RUN docker-php-ext-install simplexml
RUN docker-php-ext-install soap
RUN docker-php-ext-install zip

# gd php extension
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

# acpu extension
RUN pecl install apcu-5.1.5 && docker-php-ext-enable apcu

# mcrypt php extension
RUN pecl install mcrypt-1.0.1 && docker-php-ext-enable mcrypt

# redis
RUN pecl install redis-4.0.1 && docker-php-ext-enable redis

# xdebug
RUN pecl install xdebug-2.6.0 && docker-php-ext-enable xdebug

# Register the COMPOSER_HOME environment variable
ENV COMPOSER_HOME /composer

# Add global binary directory to PATH and make sure to re-export it
ENV PATH /composer/vendor/bin:$PATH

# Allow Composer to be run as root
ENV COMPOSER_ALLOW_SUPERUSER 1

# Setup the Composer installer
RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
    && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
    && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }"

EXPOSE 9000

CMD ["php-fpm"]
