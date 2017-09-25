# Dockerize an existing go web project

in the project folder create a file named Dockerfile and a file named docker-compose.yml

## Dockerile
```
FROM golang:latest
LABEL maintainer "Pierangelo Orizio <pierangelo1982@gmail.com>"

# for install go packages RUN go get /path
RUN go get github.com/go-sql-driver/mysql

# Copy the local package files to the container's workspace.
ADD . /go/src/github.com/pierangelo1982/myproject


# build executable
RUN go install github.com/pierangelo1982/myproject

# execute 
ENTRYPOINT /go/bin/myproject

# Document that the service listens on port 8080.
EXPOSE 8080

```
N.B: in the paths you can substitute pierangelo1982 with your github username.

For add other GO packages add in dockerfile this command (see line 5 in Dockerfile for see an example):
```
RUN go get github.com/<nameofthepackages>
```

## docker-compose.yml
```
version: '2'
services:
  app:
    build: .
    volumes:
      - .:/go/src/github.com/pierangelo1982/myproject
    expose:
      - "8080"
    ports:
      - 8080:8080

```

From the terminal, from inside the project folder launch this commands:
```
docker-compose build
```

and
```
docker-compose up -d
```

check with the browser if the app run at the address http://0.0.0.0:8080



