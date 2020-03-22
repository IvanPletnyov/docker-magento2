FROM quay.io/alexcheng1982/apache2-php7:7.3.12

LABEL maintainer="alexcheng1982@gmail.com"
LABEL php_version="7.3.12"
LABEL magento_version="2.3.3"
LABEL description="Magento 2.3.3 with PHP 7.3.12"

ENV MAGENTO_VERSION 2.3.3

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

RUN apt-get update
RUN apt-get install -y \
            git \
            curl \
            wget \
            libpng++-dev \
            libzip-dev \
            libmcrypt-dev \
            libmcrypt4 \
            libcurl3-dev \
            libfreetype6 \
            libjpeg-turbo8 \
            libjpeg-turbo8-dev \
            libfreetype6-dev \
            libicu-dev \
            libxslt1-dev \
            unzip \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install sockets \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install xsl \
    && docker-php-ext-install soap \
    && docker-php-ext-install bcmath

RUN yes '' | pecl install mcrypt-1.0.3 \
    && echo 'extension=mcrypt.so' > /usr/local/etc/php/conf.d/mcrypt.ini

RUN chsh -s /bin/bash www-data
RUN chown -R www-data:www-data /var/www

RUN a2enmod rewrite
RUN echo "memory_limit=2048M" > /usr/local/etc/php/conf.d/memory-limit.ini

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www/html

# Add cron job
#ADD crontab /etc/cron.d/magento2-cron
#RUN chmod 0644 /etc/cron.d/magento2-cron \
#    && crontab -u www-data /etc/cron.d/magento2-cron