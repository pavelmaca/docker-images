FROM php:5.6-cli

##
#RUN apt-get update && apt-get upgrade -y \
#  postgress  libpq-dev \
#  mcrypt  libmcrypt-dev \
#  mcrypt  libmcrypt-dev \
#  memcache  libmemcached-dev \
#    libssl-dev \
#    libssl-doc \
#    libsasl2-dev \
#    zlib1g-dev \
#    && docker-php-ext-install -j$(nproc) bz2 mcrypt mysqli zip

RUN apt-get update && apt-get upgrade --assume-yes \
    less \
    && rm -rf /var/lib/apt/lists/*

## PHP extensions

## iconv, mbstring
RUN docker-php-ext-install -j$(nproc) mbstring iconv

## Mysql
RUN docker-php-ext-install -j$(nproc) pdo pdo_mysql

## Xdebug
RUN pecl install xdebug-2.5.5  \
    && docker-php-ext-enable xdebug

## Redis
RUN pecl install -o -f redis \
    &&  docker-php-ext-enable redis

## BCMath
RUN docker-php-ext-install -j$(nproc) bcmath

## GD
RUN apt-get update && apt-get install --assume-yes libpng-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng12-dev \
    libmagickwand-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

## ZipArchive
RUN apt-get update && apt-get install --assume-yes zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install -j$(nproc) zip


## EXIF
RUN docker-php-ext-install -j$(nproc) exif


ENV XDEBUG_HOST=10.0.75.1

# Copy custom configuration
COPY config/xdebug-cli.ini /usr/local/etc/php/conf.d/
COPY config/php-cli.ini /usr/local/etc/php/php.ini



ENTRYPOINT ["docker-php-entrypoint"]
CMD ["php", "-a"]