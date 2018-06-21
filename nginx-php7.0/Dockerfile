FROM debian:stretch

MAINTAINER Ming-Yu,Kuo "b10500086@yuntech.edu.tw"

# Change deb source to free.nchc.org.tw
RUN sed -i 's/deb.debian.org/free.nchc.org.tw/g' \
    /etc/apt/sources.list

# RUN apt-get update
RUN apt-get update -q

# Install basic Linux tools & nginx
RUN apt-get install -y \
    sudo \
    software-properties-common \
    bzip2 \
    gnupg2 \
    apt-utils \
    apt-transport-https \
    lsb-release \
    ca-certificates \
    wget \
    curl \
    nano \
    vim \
    git \
    tar \
    unzip \
    nginx \
    tzdata \
    supervisor \
    libcurl4-openssl-dev \
    pkg-config \
    libsasl2-dev 
# Install php7.0
RUN apt-get install --no-install-recommends -y \
    php7.0 \
    php7.0-common \
    php7.0-cli \
    php7.0-curl \
    php7.0-fpm \
    php7.0-gd \
    php7.0-intl \
    php7.0-json \
    php7.0-memcached \
    php7.0-mcrypt \
    php7.0-mysql \
    php7.0-sqlite \
    php7.0-xdebug \
    php7.0-zip \
    php7.0-dev \
    php7.0-mbstring \
    php7.0-xml \
    php-pear

RUN phpdismod -s cli xdebug

# configure NGINX as non-daemon
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
# configure php-fpm as non-daemon
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.0/fpm/php-fpm.conf
# clear apt cache and remove unnecessary packages
RUN apt-get autoclean && apt-get -y autoremove

# add a phpinfo script for INFO purposes
RUN echo "<?php phpinfo();" >> /var/www/html/index.php

# NGINX mountable directories for config and logs
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx"]

# NGINX mountable directory for apps
VOLUME ["/var/www"]

# copy config file for Supervisor
COPY config/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# backup default default config for NGINX
RUN cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak

# copy local defualt config file for NGINX
COPY config/nginx/default /etc/nginx/sites-available/default

# php7.0-fpm will not start if this directory does not exist
RUN mkdir /run/php

# NGINX ports
EXPOSE 80 443

CMD ["/usr/bin/supervisord"]
