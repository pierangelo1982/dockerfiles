## Joomla Docker
```
docker run --name test-joomla -e JOOMLA_DB_HOST=localhost:3306 \
    -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=alnitek -d joomla\
```

```
docker run --name test-joomla -e JOOMLA_DB_HOST=localhost:3306 -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=alnitek -p 8082:80 -d joomla 
```

# testato
```
docker run --name test-joomla --link test-mysql:db -e JOOMLA_DB_HOST=db:3306 -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=alnitek -p 8082:80 -d joomla
```
# con volumes
```
docker run --name test-joomla --link test-mysql:db -e JOOMLA_DB_HOST=db:3306 -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=alnitek -v /Users/pierangeloorizio/Desktop/joomla-docker/joomla_www:/var/www/html  -p 8082:80 -d joomla
```

