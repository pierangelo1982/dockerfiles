## Django
### dockerize and existing django project

Inside you django prject, create this three files: 
- Dockerfile
- docker-compose.yml
- requirements.txt

In Dockerfile insert this:
```
FROM python:2.7
 RUN apt-get update -qq && apt-get install build-essential g++ flex bison gperf ruby perl \
  mysql-client \
  libsqlite3-dev libmysqlclient-dev libfontconfig1-dev libicu-dev libfreetype6 libssl-dev \
  libpng-dev libjpeg-dev python libx11-dev libxext-dev -y
 ENV PYTHONUNBUFFERED 1
 RUN mkdir /code
 WORKDIR /code
 ADD requirements.txt /code/
 COPY . /code
 RUN pip install -r requirements.txt
 ADD . /code/
```


In requirements.txt declare the  packages or libraries that you want use:

example of requirements.txt
```
Django==1.9.8
argcomplete==1.8.2
autopep8==1.2.4
beautifulsoup4==4.5.3
chardet==2.3.0
click==6.7
configparser==3.5.0
Django==1.9.8
django-appconf==1.0.1
django-carton==1.2.1
django-filer==1.2.5
django-filter==1.0.0
django-grappelli==2.9.1
django-image-cropping==1.0.4
django-mptt==0.8.6
```
In docker-compose.yml we go to insert Django, a mysqldb and phpmyadmin:

docker-compose.yml
```
version: '2'
services:
  db:
    image: mysql
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: mypassword
      MYSQL_USER: root
      MYSQL_PASSWORD: mypassword
      MYSQL_DATABASE: django-test
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      - PMA_ARBITRARY=1
    restart: always
    ports:
      - 8082:80
    volumes:
      - /sessions  
```

After that go inside this folder from the terminal and launch this command:

```
docker-compose build
```

```
docker-compose up 
```

For use Django commands:
```
docker-compose exec web python manage.py makemigrations

docker-compose exec web python manage.py migrate

docker-compose exec web python manage.py runserver

```

In Daemon Mode:
```
docker-compose up -d
```

Now chek if all run with the command:
```
docker ps
```
if all ok you shoud see something as this:
```
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                    NAMES
46fd01cbe792        djangodocker_web        "python manage.py ..."   28 minutes ago      Up 25 minutes       0.0.0.0:8000->8000/tcp   djangodocker_web_1
b1f6cacc9cae        mysql                   "docker-entrypoint..."   33 minutes ago      Up 27 minutes       0.0.0.0:3306->3306/tcp   djangodocker_db_1
bd9d381e6f89        phpmyadmin/phpmyadmin   "/run.sh phpmyadmin"     33 minutes ago      Up 27 minutes       0.0.0.0:8082->80/tcp     djangodocker_phpmyadmin_1
```

