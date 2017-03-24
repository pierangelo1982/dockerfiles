## MYSQL

docker run --name test-mysql -v /Users/pierangeloorizio/Desktop/mysql-nginx/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=alnitek -d mysql


## phpmyadmin
docker run --name test-phpmyadmin -d --link test-mysql:db -p 8084:80 phpmyadmin/phpmyadmin

