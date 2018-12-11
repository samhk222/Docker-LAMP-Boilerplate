FROM php:7.2-apache

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y

RUN apt-get -y install apt-utils iputils-ping nano wget dialog \
    build-essential git curl libcurl3 libcurl3-dev zip \
    libmcrypt-dev libsqlite3-dev libsqlite3-0 mysql-client openssl \
    zlib1g-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && rm -rf /var/lib/apt/lists/*

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install xdebug
# RUN pecl install xdebug-2.6.0
# RUN docker-php-ext-enable xdebug

RUN pecl install mcrypt-1.0.1

# Install redis
RUN pecl install redis-4.0.1
RUN docker-php-ext-enable redis

# Other PHP7 Extensions

# PHP5 Extensions
RUN docker-php-ext-install curl \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install json \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_sqlite \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
    # && pecl install xdebug-2.5.5 && docker-php-ext-enable xdebug \
    # && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini

# Para conseguir ler as variáveis de ambiente no php-cli
RUN echo 'alias ll="ls -lah"' >> ~/.bashrc

# RUN composer global require laravel/installer
#RUN export PATH=$PATH:$ADDITIONAL_PATH;

ENV AMBIENTE DEV
ENV DB_HOST SERVER-5.6-mysql
ENV DB_DATABASE wms
ENV DB_USERNAME root
ENV DB_PASSWORD password
ENV ENV_FROM Dockerfile
ENV DOCKER_WEB_PORT 8877
# ENV LARAVEL_PATH ~/.composer/vendor/bin
# Enable apache modules
RUN a2enmod rewrite headers proxy proxy_http

EXPOSE 80

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]


