FROM php:7.2-cli-alpine

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

RUN apk add --no-cache less

## PHP extensions

## iconv, mbstring
RUN docker-php-ext-install mbstring iconv

## Mysql
RUN docker-php-ext-install pdo pdo_mysql

## Xdebug	
RUN apk --no-cache add pcre-dev ${PHPIZE_DEPS} \ 
  && pecl install xdebug \
  && docker-php-ext-enable xdebug \
  && apk del pcre-dev ${PHPIZE_DEPS}

## Redis
RUN apk --no-cache add pcre-dev ${PHPIZE_DEPS} \ 
  && pecl install redis \
  && docker-php-ext-enable redis \
  && apk del pcre-dev ${PHPIZE_DEPS}

## BCMath
RUN docker-php-ext-install bcmath

## GD
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
  docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd && \
  apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

## ZipArchive
RUN apk add --no-cache zlib-dev \
    && docker-php-ext-install zip


## EXIF
RUN docker-php-ext-install exif


ENV XDEBUG_HOST=10.0.75.1

# Copy custom configuration
COPY config/xdebug-cli.ini /usr/local/etc/php/conf.d/
COPY config/php-cli.ini /usr/local/etc/php/php.ini



ENTRYPOINT ["docker-php-entrypoint"]
CMD ["php", "-a"]