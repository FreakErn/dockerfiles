
FROM nginx:alpine

ARG NGINX_PORT=80
ARG NGINX_SSL_PORT=443
RUN apk add --update --no-cache openssl && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/cert.key -out /etc/nginx/cert.crt -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"

#openssl req -nodes -newkey rsa:2048 -keyout /etc/nginx/cert.key -out /etc/nginx/cert.crt -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"

RUN echo -e "server {\n  listen 80;\n resolver 127.0.0.11 ipv6=off;\n server_name localhost 127.0.0.1;\n location / {\nproxy_pass_header Authorization;\n  proxy_pass http://HIDDEN_SERVER:$NGINX_PORT;\n  proxy_set_header Host \$host;\n  proxy_set_header X-Real-IP \$remote_addr;\n  proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;\n  proxy_http_version 1.1;\n  proxy_set_header Connection \"\";\n  proxy_buffering off;\n  client_max_body_size 0;\n  proxy_read_timeout 36000s;\n  proxy_redirect off;\n }\n}\n server {\n listen      443 ssl;\n ssl on;\n server_name localhost 127.0.0.1;\n ssl_certificate        /etc/nginx/cert.crt;\n ssl_certificate_key    /etc/nginx/cert.key;\n ssl_verify_client      off;\n location / {\n proxy_pass https://HIDDEN_SERVER:$NGINX_SSL_PORT;\n }\n}" > /etc/nginx/conf.d/default.conf
