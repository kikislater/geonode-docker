ARG NGINX_VERSION=1.26.3
FROM nginx:${NGINX_VERSION}-alpine

RUN apk add --no-cache openssl inotify-tools vim bash logrotate fail2ban curl

WORKDIR /etc/nginx/

RUN mkdir -p /etc/nginx/html \
             /etc/fail2ban/filter.d &&\
    touch /etc/nginx/html/index.html

ADD nginx.conf.envsubst nginx.https.available.conf.envsubst ./
ADD geonode.conf.envsubst ./sites-enabled/

COPY logrotate-nginx /etc/logrotate.d/nginx
COPY jail.local /etc/fail2ban/jail.d/nginx-malicious.conf
COPY filter.d/nginx-malicious.conf /etc/fail2ban/filter.d/nginx-malicious.conf

ADD docker-autoreload.sh docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
RUN chmod +x /docker-autoreload.sh
RUN chmod +x /docker-entrypoint.sh

CMD ["nginx", "-g", "daemon off;"]
