FROM php:7.2-fpm

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y
RUN apt-get install -y \
        apt-utils \
        nano \
        locate \
        curl \
        wget \
		w3m \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libxslt-dev \
        libicu-dev \
        libmcrypt-dev \
        libxml2-dev

# RUN apt-get install -y memcached php-memcached
RUN apt-get install -y memcached
RUN apt-get install -y libmemcached-dev zlib1g-dev libicu-dev
RUN git clone -b php7 https://github.com/php-memcached-dev/php-memcached /usr/src/php/ext/memcached \
    && docker-php-ext-configure /usr/src/php/ext/memcached \
        --disable-memcached-sasl \
    && docker-php-ext-install /usr/src/php/ext/memcached \
    && rm -rf /usr/src/php/ext/memcached \
	&& docker-php-ext-enable memcached

# Install memcache extension
RUN set -x \
    && apt-get install -y --no-install-recommends unzip \
    && cd /tmp \
    && curl -sSL -o php7.zip https://github.com/websupport-sk/pecl-memcache/archive/php7.zip \
    && unzip php7 \
    && cd pecl-memcache-php7 \
    && /usr/local/bin/phpize \
    && ./configure --with-php-config=/usr/local/bin/php-config \
    && make \
    && make install \
    && echo "extension=memcache.so" > /usr/local/etc/php/conf.d/ext-memcache.ini \
    && rm -rf /tmp/pecl-memcache-php7 php7.zip

RUN docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install xsl
RUN docker-php-ext-install soap
RUN docker-php-ext-install exif

## Install Redis and Xdebug
RUN pecl install xdebug-2.6.1 \
	pecl install redis \
    && docker-php-ext-enable redis xdebug

## Install imagick
RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        libmagickwand-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick
    
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

#stat memcached demon
#CMD memcached -d -m 512 -l 127.0.0.1 -p 11211 -u nobody

WORKDIR /var/www

CMD ["php-fpm"]
