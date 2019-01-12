FROM php:7.1-fpm
MAINTAINER kirito "kirito@zzz.com"
 
# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        gcc \
        make \
        automake \
        autoconf \
        re2c \
        libpcre3-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
	libxml2-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && docker-php-ext-install pcntl \
        && pecl install psr && docker-php-ext-enable psr \
        && docker-php-ext-install soap \
        && docker-php-ext-install shmop \
        && pecl install redis && docker-php-ext-enable redis \
        && pecl install swoole && docker-php-ext-enable swoole \
        && pecl install yaf && docker-php-ext-enable yaf \
        && rm -r /var/lib/apt/lists/
COPY cphalcon /home/cphalcon
RUN cd /home \
    && mv cphalcon phalcon \
    && cd phalcon/build \
    && ./install \
    && docker-php-ext-enable phalcon \
    && rm -r /home/phalcon
# 安装composer
ADD composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer
WORKDIR /var/www
# Write Permission
EXPOSE 9000
RUN usermod -u 1000 www-data

