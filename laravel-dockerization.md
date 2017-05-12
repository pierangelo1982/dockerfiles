### Dockerize an existing Laravel application with docker-compose

in the main folder of your Laravel app, create a file named Dockerfile
and insert this code:
```
FROM php:7
RUN apt-get update -y && apt-get install -y openssl zip unzip git
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install pdo pdo_mysql
WORKDIR /app
COPY . /app
RUN composer install
CMD php artisan serve --host=0.0.0.0 --port=8181
EXPOSE 8181
```
In the same main folder of Dockerfile, create a file named docker-compose.yml
and insert this code:
```
version: '2'
services:
  app:
    build: .
    ports:
      - "8009:8000"
    volumes:
      - .:/app
    env_file: .env
    working_dir: /app
    command: bash -c 'php artisan migrate && php artisan serve --host 0.0.0.0'
    depends_on:
      - db
    links:
      - db
  db:
    image: "mysql:5.7"
    environment:
      - MYSQL_ROOT_PASSWORD=yourpassword
      - MYSQL_DATABASE=yourdbname
      - MYSQL_USER=root
      - MYSQL_PASSWORD=yourpassword
    volumes:
      - ./data/:/var/lib/mysql
    ports:
      - "3306:3306"
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - 8090:80
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: yourpassword
```
Open the terminal and go inside the laravel folder, and launch this command:
```
docker.compose build
```
```
docker-compose up -d
```

if have need to create and migrate the db, or every other commands,, launch the Laravel commands in this way:
```
docker-compose run app php artisan
```
The app will available at the address http://0.0.0.0:8009


