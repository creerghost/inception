FROM alpine:3.20

RUN apk update && apk upgrade && \
    apk add --no-cache nginx openssl

RUN mkdir -p /etc/nginx/ssl && \
    openssl req -x509 -nodes -newkey rsa:2048 \
    -keyout /etc/nginx/ssl/inc.key \
    -out /etc/nginx/ssl/inc.crt \
    -subj "/CN=vlnikola.42.fr"

RUN rm -f /etc/nginx/nginx.conf

COPY conf/nginx.conf /etc/nginx/nginx.conf

RUN mkdir -p /run/nginx


RUN mkdir -p /var/www/html && echo "Inception NGINX Works!" > /var/www/html/index.html

EXPOSE 443

# set global configuration directives of nginx
# to keep docker container alive, nginx must run
# in the background
CMD ["nginx", "-g", "daemon off;"]

# verification:
# docker exec nginx ps aux -> shows pid1 as master process
# curl -k https://localhost -> Inception NGINX Works!
