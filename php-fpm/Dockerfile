FROM php:7.4-fpm


RUN  \
    # Change application source from deb.debian.org to aliyun source
    sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list

RUN set -eux; \
        \
        apt-get update; \
        apt-get install -y --no-install-recommends \
                cron \
                git \
                procps \
                libfreetype6-dev \
                libjpeg-dev \
                libpng-dev \
                libssl-dev \
                libwebp-dev \
                libzip-dev \
                openssl \
                supervisor \
                unzip \
                zip \
        ; \
        rm -r /var/lib/apt/lists/*

RUN set -eux; \
        \
        docker-php-ext-configure gd \
                --with-freetype \
                --with-jpeg \
                --with-webp \
        ; \
        docker-php-ext-install \
                gd \
                pcntl \
                pdo_mysql \
                sockets \
                zip \
        ; \
        pecl install redis && \
        docker-php-ext-enable redis && \
        pecl install swoole && \
        docker-php-ext-enable swoole

RUN set -eux; \
        \
        php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
        php composer-setup.php && \
        php -r "unlink('composer-setup.php');" && \
        mv composer.phar /usr/local/bin/composer && \
        composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

# optmize
RUN apt-get update && \
    apt-get install vim -y
#    apt-get install zsh -y && \
#    chsh -s /bin/zsh && \
#    sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

CMD ["php-fpm"]