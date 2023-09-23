# FROM php:8.0-apache
# FROM php:8.1-apache
FROM php:8.2-apache

## Install Git and other required extensions.
RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
	&& docker-php-ext-configure gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) gd \
    && apt-get install git -y \
    && apt-get install unzip -y \
	&& apt-get install libicu-dev -y

## Install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
	&& php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
	&& php composer-setup.php \
	&& php -r "unlink('composer-setup.php');"

## Install and Enable additional PHP extensions
RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache \
	&& docker-php-ext-install pdo_mysql \
	&& docker-php-ext-install intl \
	&& a2enmod rewrite \
	&& a2enmod headers \
	&& apt-get install default-mysql-client -y

## Install vim, nvm
RUN apt-get install vim -y \
	&& curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
	&& export NVM_DIR="$HOME/.nvm" \
	&& [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  \
	&& [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

## install memcache
RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev \
	&& pecl install memcached-3.2.0 \
	&& docker-php-ext-enable memcached

## install redis
RUN pecl install redis-5.3.7 \
	&& docker-php-ext-enable redis
