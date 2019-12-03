FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
    build-essential mariadb-client openssh-client libmcrypt-dev libpng-dev tzdata nodejs zip wget sqlite3 libcurl4-openssl-dev pkg-config libssl-dev \
    && cp -R /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime \
    && echo "America/Sao_Paulo" > /etc/timezone \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
RUN chmod +x /usr/bin/composer
RUN wget https://phar.phpunit.de/phpunit-7.phar \
    && chmod +x phpunit-7.phar \
    && mv phpunit-7.phar /usr/local/bin/phpunit
RUN docker-php-ext-install pdo_mysql tokenizer

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    zlib1g-dev \
    && docker-php-ext-install -j$(nproc) iconv zip bcmath \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install mongodb \
    && echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/ext-mongodb.ini

COPY uploads.ini /usr/local/etc/php/conf.d/uploads.ini

WORKDIR /srv

EXPOSE 3000
EXPOSE 6001

RUN composer global require hirak/prestissimo

RUN mkdir -p /usr/local/nvm
ENV NVM_DIR /usr/local/nvm
ENV NVM_VERSION 0.34.0

RUN curl https://raw.githubusercontent.com/creationix/nvm/v$NVM_VERSION/install.sh | bash

RUN ["/bin/bash", "-c", "source ~/.profile"]

RUN . $NVM_DIR/nvm.sh \
    && nvm install --lts node \
    && nvm alias default 'lts/*' \
    && nvm use default \
    && npm install -g yarn

RUN echo "alias art='php artisan'" >> ~/.bashrc
RUN echo "alias cache='art cache:clear && art config:clear &&  \
                       art route:clear && art view:clear'" >> ~/.bashrc

RUN echo "alias cc='composer clear-cache'" >> ~/.bashrc
RUN echo "alias cu='composer update'" >> ~/.bashrc
RUN echo "alias ci='composer install'" >> ~/.bashrc
RUN echo "alias cdo='composer dump-autoload'" >> ~/.bashrc

RUN echo "alias compile='npm run production'" >> ~/.bashrc
RUN echo "alias compile-watch='npm run watch'" >> ~/.bashrc

RUN echo "alias 777='chmod -R 777 .'" >> ~/.bashrc
RUN echo "alias 775='chmod -R 775 .'" >> ~/.bashrc
RUN echo "alias 644='chmod -R 644 .'" >> ~/.bashrc