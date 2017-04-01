### Django
## clean version

Create a new empty folder.

In the empty folder create a file named Dockerfile

In Dockerfile insert this:
```
FROM python:2.7
 ENV PYTHONUNBUFFERED 1
 RUN mkdir /code
 WORKDIR /code
 ADD requirements.txt /code/
 RUN pip install -r requirements.txt
 ADD . /code/
```

In the same folder create a file named requirements.txt and put all library/packages that you have need... (same things of the command pip install)

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
Now create a file named docker-compose.yml
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
docker-compose up -d
```

```
docker-compose run web django-admin.py startproject composeexample .

```

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

