###LETSECRYPT

ottieni i certificati tramite il docker di certbot
```
sudo docker run -it --rm -p 443:443 -p 83:80 --name certbot \
            -v "/home/strategylab/certificati/letsencrypt:/etc/letsencrypt" \
            -v "/home/strategylab/certificati-lib/:/var/lib/letsencrypt" \
            certbot/certbot certonly
```

siccome non so come creare cartella in un docker già esistente, condivido i certificati nel volume della cartella home
di nginx

```
docker run --name docker-nginx -p 80:80 -p 443:443 -v /home/strategylab/nginx-config/default.conf:/etc/nginx/conf.d/default.conf -v /home/strategylab/nginx-config/nginx.conf:/etc/nginx/nginx.conf -v /home:/home -d nginx
```

poi inserisco i certicati nell'nginx default.

```
ssl on;
ssl_certificate /home/strategylab/certificati/letsencrypt/live/strategylab.it/fullchain.pem;
ssl_certificate_key /home/strategylab/certificati/letsencrypt/live/strategylab.it/privkey.pem;
```
