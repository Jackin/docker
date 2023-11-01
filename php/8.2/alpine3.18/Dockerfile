FROM php:8.2-fpm-alpine3.18


# 安装没有依赖的扩展
RUN  docker-php-ext-install opcache bcmath exif pdo_mysql

# 安装soap
RUN set -eux; \
    apk add --no-cache --virtual .soap-deps libxml2-dev && \
    docker-php-ext-configure soap && \
    docker-php-ext-install soap && \
    apk del --no-network .soap-deps

# 安装 gettext
RUN set -eux; \
    apk add --no-cache --update libintl && \
    apk add --no-cache --virtual .gettext-deps icu-dev gettext-dev && \
    docker-php-ext-install gettext && \
    apk del --no-network .gettext-deps

# 安装 sockets
RUN set -eux; \
    apk add --no-cache --virtual .sockets-deps linux-headers && \
    docker-php-ext-install sockets && \
    apk del --no-network .sockets-deps

# 安装zip
RUN set -eux; \
    apk add --no-cache --update libzip && \
    apk add --no-cache --virtual .zip-deps libzip-dev && \
    docker-php-ext-install zip && \
    apk del --no-network .zip-deps

# 安装 gd
RUN set -eux; \
    apk add --no-cache --update libpng libwebp libjpeg freetype zlib && \
    apk add --no-cache --virtual .gd-deps freetype-dev libjpeg-turbo-dev libwebp-dev zlib-dev libpng-dev && \
    docker-php-ext-configure gd --with-webp --with-freetype --with-jpeg && \
    docker-php-ext-install gd && \
    apk del --no-network .gd-deps

# 安装pecl扩展
RUN set -eux; \
    apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS && \
    pecl install psr-1.2.0 xhprof-2.3.9 igbinary-3.2.14 msgpack-2.2.0 \
    pecl clear-cache && \
    docker-php-ext-enable psr xhprof igbinary msgpack && \
    rm -rf "$(pecl config-get temp_dir)" "$(pecl config-get download_dir)/" && \
    apk del --no-network .phpize-deps

# 安装pecl扩展 redis-6.0.2
RUN set -eux; \
    apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS && \
    pecl install --nobuild redis-6.0.2 && \
    docker-php-ext-configure "$(pecl config-get temp_dir)/redis" --enable-redis-igbinary --enable-redis-msgpack && \
    docker-php-ext-install "$(pecl config-get temp_dir)/redis" && \
    pecl clear-cache && \
    rm -rf "$(pecl config-get temp_dir)" "$(pecl config-get download_dir)/" && \
    apk del --no-network .phpize-deps

# 安装pecl扩展 yac yaconf
RUN set -eux; \
	apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS && \
	pecl install yaconf-1.1.2 yac-2.3.1 && \
	docker-php-ext-enable yac yaconf && \
    pecl clear-cache && \
	rm -rf "$(pecl config-get temp_dir)" "$(pecl config-get download_dir)/" && \
	apk del --no-network .phpize-deps

# 安装pecl扩展 phalcon-5.4.0
RUN set -eux;\
    apk add --no-cache --virtual .phalcon-deps $PHPIZE_DEPS pcre-dev gd-dev && \
    pecl channel-update pecl.php.net && \
    pecl install phalcon-5.4.0 && \
    docker-php-ext-enable phalcon && \
    pecl clear-cache && \
    rm -rf "$(pecl config-get temp_dir)" "$(pecl config-get download_dir)/" && \
    apk del --no-network .phalcon-deps