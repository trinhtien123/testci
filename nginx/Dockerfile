# create mkcert service to generate local certificate 
FROM golang:1.12-alpine as mkcert-service

RUN apk --no-cache add git

RUN cd /go && \
    go get -u github.com/FiloSottile/mkcert && \
    cd src/github.com/FiloSottile/mkcert && \
    go build -o /bin/mkcert

WORKDIR /root/.local/share/mkcert

RUN mkcert -install 
RUN mkcert localhost 127.0.0.1 *.local
RUN mkcert -CAROOT |  ls -la

#
# main container
#
FROM nginx:latest

RUN rm /etc/nginx/conf.d/default.conf

COPY ./vhosts/* /etc/nginx/conf.d/

# copy certs
COPY --from=mkcert-service /root/.local/share/mkcert /certs
