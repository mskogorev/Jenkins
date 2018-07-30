#Выбираем образ
FROM ubuntu:16.04
#Отключаем интерактивный режим
ENV DEBIAN_FRONTEND noninteractive
#Устанавливаем минимальный набор необходимых пакетов
RUN apt-get update \
    && apt-get install -y --no-install-recommends apt-utils libssl1.0.0 locales tzdata gettext-base \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
    && ln -snf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
ENV LANG en_US.utf8

# Nginx
COPY ./configs /tmp
RUN apt-get install -f -y /tmp/*.deb; rm -f /tmp/*.deb \
    && cp -rf /tmp/etc_nginx/* /etc/nginx \
    && ln -fs /etc/nginx/sites-available/default /etc/nginx/sites-enabled/ \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /run/nginx/*
