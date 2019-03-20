FROM php:7.2-fpm
MAINTAINER ma "252119596@qq.com"

ENV TZ "Asia/Shanghai"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli \
	#安装igbinary扩展
	&& pecl install -o -f igbinary \ 
	&& rm -rf /tmp/pear \ 
	&& docker-php-ext-enable igbinary \
	#安装redis扩展
	&& pecl install -o -f redis \ 
	&& rm -rf /tmp/pear \ 
	&& docker-php-ext-enable redis \
	#安装mysql扩展
	&& docker-php-ext-install pdo_mysql \
	#安装swoole1.10.1
	&& tar -xzf /tmp/swoole-src-4.3.1.tar.gz -C /usr/src/swoole --strip-components=1 \
	&& phpize \
	&& ./configure \
	&& make \
	&& make install \
	&& docker-php-ext-enable swoole \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -f /tmp/swoole-src-4.3.1.tar.gz \
	&& rm -r /usr/src/swoole \

RUN mkdir -p /usr/src/php/ext/redis \
    && curl -L https://github.com/phpredis/phpredis/archive/4.0.0.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
    && echo 'redis' >> /usr/src/php-available-exts \
    && docker-php-ext-install redis