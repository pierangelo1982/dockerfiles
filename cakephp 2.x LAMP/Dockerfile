FROM php:5-apache
MAINTAINER pierangelo orizio <pierangelo1982@gmail.com>

WORKDIR /var/www/html

COPY ./default.conf /etc/apache2/sites-available/000-default.conf

RUN docker-php-ext-install pdo pdo_mysql

RUN apt-get update && \
  apt-get -y install curl git libicu-dev libpq-dev zlib1g-dev zip && \
  docker-php-ext-install intl mbstring pcntl pdo_mysql pdo_pgsql zip && \
  usermod -u 1000 www-data && \
  usermod -a -G users www-data && \
  chown -R www-data:www-data /var/www && \
  curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
  a2enmod rewrite


COPY . /var/www/html
