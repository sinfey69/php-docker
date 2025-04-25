FROM php:5.6.40-fpm

WORKDIR /home

# 设置代理环境变量
ARG HTTP_PROXY
ARG HTTPS_PROXY
ENV http_proxy=$HTTP_PROXY
ENV https_proxy=$HTTPS_PROXY
ENV NO_PROXY=localhost,127.0.0.1

# 测试代理是否生效
# RUN curl -I https://www.google.com

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list && \
    echo "deb-src http://archive.debian.org/debian stretch main" >> /etc/apt/sources.list && \
    echo "Acquire::Check-Valid-Until false;" > /etc/apt/apt.conf.d/99no-check-valid-until && \
    apt-get update && \
    apt-get install -y libpng-dev libjpeg-dev libfreetype6-dev zlib1g-dev libzip-dev unzip

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd

RUN docker-php-ext-configure zip && \
    docker-php-ext-install zip

# 清除代理环境变量，否则构建的镜像里会带上这代理（也可以使用多阶构建来避免镜像代理污染）
ENV http_proxy=
ENV https_proxy=

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

RUN rm -rf /home/*