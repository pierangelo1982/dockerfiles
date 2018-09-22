# dockerize an existing hugo.io app.


from inside the app folder launch this:
```
docker build -t my-hugo .
````


docker run --name my-blog -v ~/Scrivania/blog:/go/src/blog -p 1313:1313 -d my-hugo