###NGINX

### config
```
docker run --name nginx-proxy -v /host/path/nginx.conf:/etc/nginx/nginx.conf:ro -d -p 8080:80 nginx
```
### run - personalized for my mac
(crea la cartella dove caricare index e siti vari)
```
docker run --name docker-nginx -p 80:80 -d -v /Users/pierangeloorizio/Desktop/nginx-test:/usr/share/nginx/html nginx
```
(copia dal docker creato il file di configurazione)
```
docker cp docker-nginx:/etc/nginx/conf.d/default.conf default.conf
```

rimuovi i docker e creane uno nuovo che punti al file default.conf personalizzato copiato in precedenza

```
sudo docker run --name docker-nginx -p 80:80 -v ~/docker-nginx/html:/usr/share/nginx/html -v ~/docker-nginx/default.conf:/etc/nginx/conf.d/default.conf -d nginx
```

```
docker run --name docker-nginx -p 80:80 -v ~/docker-nginx/html:/usr/share/nginx/html -v /Users/pierangelorizio/Desktop/nginx-test/docker-nginx/default.conf:/etc/nginx/conf.d/default.conf -d nginx 
```

```
docker run --name docker-nginx -p 80:80 -v /Users/pierangeloorizio/Desktop/nginx-test/default.conf:/etc/nginx/conf.d/default.conf -d nginx
```

### ottenere anche file di configurazione nginx.conf
copia dall'istanza il file nginx.conf

```
cp docker-nginx:/etc/nginx/nginx.conf /Users/pierangeloorizio/Desktop/nginx-test/nginx.conf
```

```
docker run --name docker-nginx -p 80:80 -v /home/strategylab/nginx-config/default.conf:/etc/nginx/conf.d/default.conf -v /home/strategylab/nginx-config/nginx.conf:/etc/nginx/nginx.conf -d nginx
```

```
docker run --name docker-nginx -p 80:80 -v ~/docker-nginx/html:/usr/share/nginx/html -v ~/docker-nginx/default.conf:/etc/nginx/conf.d/default.conf -v ~/docker-nginx/nginx.conf:/etc/nginx/nginx.conf -d nginx
```
