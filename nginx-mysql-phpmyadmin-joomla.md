# NGINX
docker run --name docker-nginx -p 80:80 -v /my/local/path/nginx-config/default.conf:/etc/nginx/conf.d/default.conf -d nginx

# MYSQL
docker run --name test-mysql -v /my/local/path/test-database/datadir:/var/mysql -e MYSQL_ROOT_PASSWORD=strategylabdb2017 -d mysql

# phpMyAdmin
docker run --name test-phpmyadmin -d --link test-mysql:db -p 8082:80 phpmyadmin/phpmyadmin

# JOOMLA
docker run --name test-joomla --link docker-nginx --link test-mysql:db -e JOOMLA_DB_HOST=db:3306 -e JOOMLA_DB_USER=root -e JOOMLA_DB_PASSWORD=mypassword -v /my/local/path/test-joomla/joomla_www:/var/www/html -p 8081:80 -d joomla



## for restart the instance
docker start dockernginx

docker start strategy-mysql

docker start strategy-phpmyadmin

docker start strategy-lab


###+++++ Porte e Servizi ++++++++
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                         NAMES

1f7db290366b        nginx                   "nginx -g 'daemon off"   5 hours ago         Up 58 seconds       0.0.0.0:80->80/tcp, 443/tcp   docker-nginx

969e42300600        joomla                  "/entrypoint.sh apach"   5 hours ago         Up 5 hours          0.0.0.0:8081->80/tcp          test-joomla

f2e4659b1b3c        phpmyadmin/phpmyadmin   "/run.sh phpmyadmin"     5 hours ago         Up 5 hours          0.0.0.0:8082->80/tcp          test-phpmyadmin

42931b06703f        mysql                   "docker-entrypoint.sh"   5 hours ago         Up 5 hours          3306/tcp                      test-mysql
