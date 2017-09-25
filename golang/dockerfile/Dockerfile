FROM golang:latest
LABEL maintainer "Pierangelo Orizio <pierangelo1982@gmail.com>"

# for install go packages
RUN go get github.com/go-sql-driver/mysql

# Copy the local package files to the container's workspace.
ADD . /go/src/github.com/pierangelo1982/myproject


# build executable
RUN go install github.com/pierangelo1982/myproject

# execute 
ENTRYPOINT /go/bin/myproject

# Document that the service listens on port 8080.
EXPOSE 8080
