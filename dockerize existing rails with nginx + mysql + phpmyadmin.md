## ======== NGINX =========

create ngix instance:
```
docker run --name nginx-solinas -p 80:80 -v /Users/pierangeloorizio/Desktop/solinas-docker/nginx-solinas/default.conf:/etc/inx/conf.d/default.conf -v /Users/pierangeloorizio/Desktop/nginx-test:/usr/share/nginx/html -d nginx
```
```
docker start nginx-solinas
```

## ======= MySql =========

create mysql instance:
```
docker run --name mysql-solinas -v /Users/pierangeloorizio/Desktop/solinas-docker/mysql/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=alnitek -d mysql
```
```
docker start mysql-solinas 
```

## ====== phpmyadmin =======
```
docker run --name phpmyadmin-solinas -d --link mysql-solinas:db -p 8084:80 phpmyadmin/phpmyadmin
```
```
docker start phpmyadmin-solinas
```
===== RAILS =======

Create Dockerfile in the app folder
```
FROM ruby:2.3.3
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /usr/src/pdiesel_backend
WORKDIR /usr/src/pdiesel_backend
ADD Gemfile /usr/src/pdiesel_backend/Gemfile
ADD Gemfile.lock /usr/src/pdiesel_backend/Gemfile.lock
RUN bundle install
ADD . /usr/src/pdiesel_backend
RUN RAILS_ENV=production bundle exec rake assets:precompile --trace
```

create docker-compose.yml in folder

```
version: '2'
services:
  app:
    build: .
    command: bundle exec puma -C config/puma.rb
    volumes:
      - .:/usr/src/pdiesel_backend
    expose:
      - "3000"
    environment:
      RACK_ENV: production
      RAILS_ENV: production
    ports:
        - 3002:3000
    external_links:
      - mysql-solinas:db
    network_mode: bridge
```


entra nella cartella principale:
```
docker-compose build
```
```
docker-compose up -d
```
```
docker-compose run app rake db:create db:migrate RAILS_ENV=production
```
```
docker-compose run app rake assets:precompile --trace RAILS_ENV=production
```
