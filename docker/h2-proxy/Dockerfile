FROM alpine:latest

RUN apk add --no-cache openssl

# Use this self-generated certificate only in dev, IT IS NOT SECURE!
RUN openssl genrsa -des3 -passout pass:NotSecure -out cert.pass.key 2048
RUN openssl rsa -passin pass:NotSecure -in cert.pass.key -out cert.key
RUN rm cert.pass.key
RUN openssl req -new -passout pass:NotSecure -key cert.key -out cert.csr \
    -subj '/C=SS/ST=SS/L=Gotham City/O=Bolt Dev/CN=localhost'
RUN openssl x509 -req -sha256 -days 365 -in cert.csr -signkey cert.key -out cert.crt

# FROM nginx:1.17-alpine
# added
FROM openresty/openresty:1.17.8.2-alpine AS nginx

RUN echo -e "env UPSTREAM;\n$(cat /usr/local/openresty/nginx/conf/nginx.conf)" > /usr/local/openresty/nginx/conf/nginx.conf
COPY conf.d/default.conf /etc/nginx/conf.d/default.conf
RUN mkdir -p /etc/nginx/ssl/
COPY --from=0 cert.key cert.crt /etc/nginx/ssl/

# End
# RUN mkdir -p /etc/nginx/ssl/
# COPY --from=0 cert.key cert.crt /etc/nginx/ssl/
# COPY conf.d /etc/nginx/conf.d/
