FROM php:7.3-fpm
RUN usermod -u 1000 www-data

#RUN usermod -u 1000 www-data
#RUN chown -R www-data /usr/share
#RUN sed -ri 's/^www-data:x:82:82:/www-data:x:1000:50:/' /etc/passwd
#RUN chown -R www-data:www-data /usr/share/nginx/html

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN apt-get update && apt-get install -y \
        zip \
        unzip\
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
        libzip-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-webp-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install \
           gd \
           mysqli \
           pdo \
           pdo_mysql \
           mbstring \
           tokenizer \
           opcache \
           exif \
           zip \
           pcntl

RUN pecl install redis \
    && pecl install xdebug \
    && pecl install mcrypt \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-enable redis xdebug


RUN curl -sS https://getcomposer.org/installer | \
            php -- --install-dir=/usr/bin/ --filename=composer

#COPY start.sh /usr/bin
#
#CMD ["bash", "/usr/bin/start.sh"]
#
COPY supervisord.conf /etc/supervisor
COPY config/supervisor/* /etc/supervisor/conf.d

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]