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

