FROM php:7.0.6-fpm

#Install Composer globally
RUN curl -s -o /usr/local/bin/composer https://getcomposer.org/composer.phar && \
    chmod 0755 /usr/local/bin/composer

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install json
RUN docker-php-ext-install mbstring

RUN apt-get update && apt-get install -y zlib1g-dev && docker-php-ext-install zip

# Install mcrypt
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libldap2-dev  \
        libpng12-dev \
    && docker-php-ext-configure gd --enable-gd-native-ttf --with-jpeg-dir=/usr/lib/x86_64-linux-gnu --with-png-dir=/usr/lib/x86_64-linux-gnu --with-freetype-dir=/usr/lib/x86_64-linux-gnu \
    && docker-php-ext-install gd \
    && docker-php-ext-install -j$(nproc) iconv mcrypt

# Install phpredis 2.2.7
RUN apt-get update \
&& apt-get install git -y -q \
&& git clone -b php7 https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis \
&& docker-php-ext-install redis

RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h && \
 docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu

RUN docker-php-ext-install ldap

# Install PHP-CLI 7, some PHP extentions and some useful Tools with APT
RUN apt-get update && apt-get install -y --force-yes \
        sqlite3 \
        vim \
        nodejs \
        nodejs-dev \
        npm

RUN git clone https://github.com/hnw/php-timecop.git && cd php-timecop && phpize && ./configure && make && make install
RUN echo "extension=timecop.so" >> /usr/local/etc/php/conf.d/timecop.ini


# Install gulp and bower with NPM
RUN npm install -g \
    gulp \
    bower

# Add a symbolic link for Node
RUN ln -s /usr/bin/nodejs /usr/bin/node

# Add an aliases
RUN echo "alias phpunit='./vendor/bin/phpunit'" >> ~/.bashrc
RUN echo "alias dump='composer dumpautoload'" >> ~/.bashrc
RUN echo "alias refresh='php artisan migrate:refresh --seed'" >> ~/.bashrc
# Source the bash
RUN . ~/.bashrc

# add cron jobs

RUN apt-get update && apt-get -y install cron

ADD ehos.cron /etc/cron.d/ehos.cron
RUN chmod 0644 /etc/cron.d/ehos.cron
RUN touch /var/log/cron.log
RUN service cron start
