FROM php:7.2-fpm

RUN pecl install redis \
    && pecl install xdebug \
    && docker-php-ext-enable redis xdebug

RUN apt-get update && apt-get install -y \
        supervisor \
        curl \
        wget \
        git \
        libxslt-dev \
        libicu-dev \
        libmcrypt-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libwebp-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-webp-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install \
           mbstring \
           pdo \
           pdo_mysql

#COPY start.sh /usr/bin
#
#CMD ["bash", "/usr/bin/start.sh"]
#
COPY supervisord.conf /etc/supervisor
COPY config/supervisor/* /etc/supervisor/conf.d

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]