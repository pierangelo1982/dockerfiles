## cake 2.x (LAMP) docker config

# mysql
```
docker run --name docker-mysql -v /home/docker/mysql/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=alnitek82 --restart always -d mysql
```
# phpmyadmin
```
docker run --name docker-phpmyadmin -d --link docker-mysql:db -p 8081:80 -d phpmyadmin/phpmyadmin
```

# file in root cakephp folder:
# Dockerfile

```
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
```

# docker-compose.yml

```
version: '2'
services:
  web:
    build: .
    ports:
      - "8090:80"
    volumes:
      - ~/Scrivania/my-docker/glocalnow-docker/glocalnow:/var/www/html
    external_links:
      - docker-mysql:db
    network_mode: bridge

```

# default.conf

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/app/webroot

        <Directory /var/www/html/app/webroot>
                Options FollowSymLinks
                AllowOverride all
                Require all granted
        </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

```

# cmd:
```
docker-compose build
```

```
docker-compose up -d
```
