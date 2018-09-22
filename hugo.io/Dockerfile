FROM golang:latest
LABEL maintainer "Pierangelo Orizio <pierangelo1982@gmail.com>"


# go in src go directory
WORKDIR /go/src

# download hugo
RUN go get github.com/gohugoio/hugo

RUN hugo new site blog

WORKDIR /go/src/blog

RUN git init;\
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke;\
	echo 'theme = "ananke"' >> config.toml

RUN chmod -R 7777 /go/src/blog

EXPOSE 1313


ENV HUGO_BASE_URL http://localhost:1313
CMD hugo server -b ${HUGO_BASE_URL} --bind=0.0.0.0