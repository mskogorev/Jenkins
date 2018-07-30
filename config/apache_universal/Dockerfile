#Выбираем образ
FROM ubuntu:16.04
#Отключаем интерактивный режим
ENV DEBIAN_FRONTEND noninteractive
#Устанавливаем минимальный набор необходимых пакетов
RUN apt-get update \
    && apt-get install -y --no-install-recommends apt-utils libssl1.0.0 locales tzdata gettext-base curl zip unzip git openssl ca-certificates \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
    && ln -snf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
ENV LANG en_US.utf8

# Установка apache2
RUN apt-get update --fix-missing && apt-get install -y --no-install-recommends apache2 libapache2-mod-php libapache2-mod-php7.0 libapache2-mod-rpaf libapache2-mod-security2 libapache2-modsecurity
# Установка php и модулей
RUN apt-get update --fix-missing && apt-get install -y --no-install-recommends php7.0 php7.0-common php7.0-curl php7.0-gd php7.0-xml php7.0-mbstring php7.0-mcrypt php7.0-imap php7.0-mysql php-memcached php7.0-dev libphp7.0-embed php-xdebug php7.0-cli php-geoip php-igbinary php-redis php7.0-zip pkg-php-tools dh-php php7.0-soap php-mbstring \
 javascript-common
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && composer global require phpunit/phpunit 6.5.5 --no-progress --no-scripts --no-interaction \
    && mv /root/.composer/vendor /usr \
    && ln -s /usr/vendor/bin/phpunit /usr/local/bin/ \
    && rm -rf /root/.composer/

#Включение модулей апача и  пхп
RUN a2enmod rewrite expires include remoteip; a2dismod rpaf
#Копирование конфигурационных файлов
COPY ./configs /tmp
# -- установка модуля imagick и haproxy
RUN apt-get install -f -y --no-install-recommends /tmp/php-imagick_3.4.3RC1-1_amd64.deb \
    && apt-get -y --no-install-recommends install haproxy
# Настройка образа
RUN cp -rf /tmp/etc_php/* /etc/php/7.0/ \
    && cp -rf /tmp/etc_haproxy/*.cfg /etc/haproxy/ \
    && mkdir -p /var/run/haproxy \
    && rm -f /etc/apache2/sites-available/* \
    && rm -f /etc/apache2/sites-enabled/* \
    && cp -rf /tmp/etc_apache/* /etc/apache2/ \
    && ln -s /etc/apache2/sites-available/* /etc/apache2/sites-enabled/ \
    && cp -rf /tmp/bin_sendmail/* /bin/ \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /run/apache2/*
