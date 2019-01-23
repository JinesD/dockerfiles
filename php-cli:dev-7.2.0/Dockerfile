FROM php:7.2-cli-alpine3.6

ENV TIMEZONE=Asia/Shanghai
ENV PHP_MEMORY_LIMIT=512M
ENV PHP_EXT_SWOOLE=swoole-1.9.23

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
&& apk update \
&& apk --update --repository=http://dl-4.alpinelinux.org/alpine/edge/testing add hiredis-dev libmcrypt-dev gmp-dev icu-dev linux-headers musl --virtual .phpize-deps $PHPIZE_DEPS \
