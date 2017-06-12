## PRESTAHOP

### docker-compose-yml


```
version: '2'

services:

  app:
    image: prestashop/prestashop:1.7
    ports:
      - "8080:80"
    volumes:
      - ~/Scrivania/my-docker/ilprofumodelpassato-docker/ilprofumodelpassato:/var/www/html
      - ~/Scrivania/my-docker/ilprofumodelpassato-docker/img:/var/www/html/img
    external_links:
      - docker-mysql:db
    network_mode: bridge
    restart: always
```
