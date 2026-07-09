FROM nginx:1.28-alpine

RUN rm -f /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html

EXPOSE 80
