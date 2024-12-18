FROM php:5.6.40-fpm

WORKDIR /home

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list && \
    echo "deb-src http://archive.debian.org/debian stretch main" >> /etc/apt/sources.list && \
    echo "Acquire::Check-Valid-Until false;" > /etc/apt/apt.conf.d/99no-check-valid-until && \
    apt-get update && \
    apt-get install -y libpng-dev libjpeg-dev libfreetype6-dev && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd

# 清理安装包
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# install and enable phalcon
COPY ./cphalcon-phalcon-v2.0.7.tar.gz .

RUN tar zxf cphalcon-phalcon-v2.0.7.tar.gz

RUN cd cphalcon-phalcon-v2.0.7/build && ./install

RUN docker-php-ext-enable phalcon

# install and enable redis
COPY ./phpredis-2.2.7.tar.gz .

RUN tar zxf phpredis-2.2.7.tar.gz

RUN cd phpredis-2.2.7 && /usr/local/bin/phpize && ./configure -with-php-config=/usr/local/bin/php-config && make && make install && make test

RUN docker-php-ext-enable redis

# install pdo_mysql
RUN docker-php-ext-install pdo pdo_mysql

# install xdebug
COPY ./xdebug-2.5.5.tgz .

RUN tar zxf xdebug-2.5.5.tgz

RUN cd xdebug-2.5.5/ && /usr/local/bin/phpize && ./configure -with-php-config=/usr/local/bin/php-config && make && make install && make test

RUN docker-php-ext-enable xdebug