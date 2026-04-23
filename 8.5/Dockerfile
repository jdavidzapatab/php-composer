FROM php:8.5-cli-bookworm

LABEL maintainer="David Zapata <jdavid.zapatab@gmail.com>"

# Install-php-extensions script
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Fix CVEs (specifically nghttp2 CVE-2026-27135) and install PHP extensions in a single layer
RUN set -eux; \
    chmod +x /usr/local/bin/install-php-extensions; \
    apt-get update; \
    apt-get upgrade -y; \
    echo "deb http://deb.debian.org/debian sid main" > /etc/apt/sources.list.d/sid.list; \
    apt-get update; \
    apt-get install -y --no-install-recommends -t sid libnghttp2-14; \
    rm /etc/apt/sources.list.d/sid.list; \
    apt-get update; \
    install-php-extensions \
        xdebug \
        gd \
        zip \
        bcmath \
        pdo_mysql \
        pdo_pgsql \
        soap \
        redis \
        pcntl \
        mongodb \
        sockets; \
    apt-get purge -y --auto-remove gcc g++ make cpp binutils; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/* /usr/src/php.tar.xz

WORKDIR /var/www
COPY . /var/www
VOLUME /var/www

EXPOSE 80 8000

CMD ["bash"]
