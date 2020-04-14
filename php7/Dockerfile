FROM php:7.2-fpm

# 官方版本默认安装扩展: 
# Core, ctype, curl
# date, dom
# fileinfo, filter, ftp
# hash
# iconv
# json
# libxml
# mbstring, mysqlnd
# openssl
# pcre, PDO, pdo_sqlite, Phar, posix
# readline, Reflection, session, SimpleXML, sodium, SPL, sqlite3, standard
# tokenizer
# xml, xmlreader, xmlwriter
# zlib

# 更新为国内镜像
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
    && echo 'deb http://mirrors.aliyun.com/debian/ buster main non-free contrib' > /etc/apt/sources.list \
    && echo 'deb http://mirrors.aliyun.com/debian-security buster/updates main' >> /etc/apt/sources.list \
    && echo 'deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib' >> /etc/apt/sources.list \
    && echo 'deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib' >> /etc/apt/sources.list \
    && apt-get update

RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS"

RUN apt-get install -y --no-install-recommends curl libicu-dev libmemcached-dev zlib1g-dev libz-dev libpq-dev \
        libjpeg-dev libpng-dev libfreetype6-dev libssl-dev libmcrypt-dev libxml2-dev libbz2-dev libjpeg62-turbo-dev \
        libmagickwand-dev libmagickcore-dev libcurl4 libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/* \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime


# bcmath bz2 calendar dba  exif gettext sockets wddx xsl zip soap
# xmlrpc, mysqli, pcntl, pdo_mysql, shmop, sysvmsg, sysvsem, sysvshm 扩展

# Install various PHP extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) bcmath calendar exif gettext sockets dba mysqli pdo_mysql shmop sysvmsg sysvsem \
    sysvshm iconv bz2 xmlrpc wddx zip soap gd \
    # imagick 扩展
    && pecl install imagick && docker-php-ext-enable imagick \
    # mcrypt 扩展
    && pecl install mcrypt && docker-php-ext-enable mcrypt \
    # Memcached 扩展 
    && pecl install memcached && docker-php-ext-enable memcached \ 
    # Reids 扩展
    && pecl install redis && docker-php-ext-enable redis \
    # Mongodb 扩展
    && pecl install mongodb && docker-php-ext-enable mongodb \
    # msgpack 扩展
    && pecl install msgpack && docker-php-ext-enable msgpack \
    # raphf 扩展
    && pecl install raphf && docker-php-ext-enable raphf \
    # propro 扩展
    && pecl install propro && docker-php-ext-enable propro \
    # http 扩展
    && pecl install pecl_http && docker-php-ext-enable http \
    # yaf 扩展
    && pecl install yaf && docker-php-ext-enable yaf \
    # opcache 扩展 
    && docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install opcache \
    && pecl clear-cache


# composer install
RUN cd / \
    && php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php \
    && php composer-setup.php  --filename=composer  --install-dir=/usr/local/bin/ \
    && rm composer-setup.php


# 解决时区问题
ENV TZ "Asia/Shanghai"

# 终端设置
# 执行php-fpm时，默认值是dumb，这时在终端操作时可能会出现：terminal is not fully functional
ENV TERM xterm

EXPOSE 9000