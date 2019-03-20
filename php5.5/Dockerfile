
FROM php:5.5-fpm

MAINTAINER zengyu 284141050@qq.com


# env
ENV LANG C.UTF-8

# install some apps
RUN echo "deb http://deb.debian.org/debian jessie main" >/etc/apt/sources.list \
    && echo "deb http://security.debian.org/debian-security jessie/updates main" >>/etc/apt/sources.list \
    && echo "deb http://deb.debian.org/debian jessie-updates main" >>/etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/debian jessie main non-free contrib" >>/etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/debian jessie main non-free contrib" >>/etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/debian jessie-updates main non-free contrib" >>/etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/debian jessie-updates main non-free contrib" >>/etc/apt/sources.list \
    && apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    unzip unrar apt-utils procps inetutils-ping \
    && apt-get clean && apt-get autoclean

# add a default user named debian
RUN useradd debian -s /bin/bash -m \
    && echo "debian  ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# install composer
RUN cd /usr/local/bin \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar composer \
    && echo "update env" \
    && echo "export PATH=\$PATH:/root/.composer/vendor/bin" >> /root/.bashrc \
    && echo "export PATH=\$PATH:/root/.composer/vendor/bin" >> /root/.profile \
    && echo "export PATH=\$PATH:/root/.composer/vendor/bin" >> /etc/profile

# install  php ext
RUN docker-php-ext-install pdo_mysql mbstring bcmath


# phalcon
RUN mkdir /tmp/phalcon -p && cd /tmp/phalcon \
    && curl -L https://github.com/phalcon/cphalcon/archive/v3.0.4.tar.gz -o phalcon.tar.gz \
    && tar -zxvf phalcon.tar.gz \
    && cd /tmp/phalcon/cphalcon-3.0.4/build \
    && ./install \
    && docker-php-ext-enable phalcon \
    && rm -rf /tmp/phalcon

# 以后可以用遍历文件夹的方式
ADD extension /tmp/extension

# apcu
RUN php /tmp/extension/ExtInstaller.php -n apcu

# swoole
RUN php /tmp/extension/ExtInstaller.php -n swoole

# xdebug
RUN php /tmp/extension/ExtInstaller.php -n xdebug

