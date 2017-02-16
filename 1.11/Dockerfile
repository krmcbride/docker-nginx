FROM krmcbride/debian:jessie

ENV NGINX_VERSION 1.11.10-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
    && echo "deb http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y \
                        ca-certificates \
                        nginx=${NGINX_VERSION} \
                        nginx-module-xslt \
                        nginx-module-geoip \
                        nginx-module-image-filter \
                        nginx-module-perl \
                        nginx-module-njs \
                        gettext-base \
    && rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

COPY nginx.conf /etc/nginx/nginx.conf
COPY mime.types /etc/nginx/mime.types

# SSL setup (the commented lines or would need to be done in a child Dockerfile)
RUN mkdir -p /etc/nginx/ssl
COPY ssl.conf /etc/nginx/ssl/ssl.conf
#COPY ca-certs.pem /etc/nginx/ssl/ca-certs.pem
#COPY server.crt /etc/nginx/ssl/server.crt
#COPY server.key /etc/nginx/ssl/server.key
#RUN openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048
#RUN ln -s /etc/nginx/ssl/ssl.conf /etc/nginx/conf.d/ssl.conf

# Prep a directory for "global" static assets like favicon.ico and apple-touch-icon.png
RUN mkdir -p /var/www/static
#COPY favicon.ico /var/www/static/favicon.ico
#COPY apple-touch-icon.png /var/www/static/apple-touch-icon.png

EXPOSE 80 443

CMD ["nginx"]
