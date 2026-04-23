FROM php:5.6-cli

LABEL maintainer="David Zapata <jdavid.zapatab@gmail.com>"

# Install-php-extensions script
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install composer (Composer 2.2 is the last version supporting PHP 5.6)
COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer

# Install PHP extensions in a single layer
    # Note: mcrypt is required for 5.6 as per instructions
    # Switch to archive.debian.org for EOL Stretch
RUN set -eux; \
    sed -i 's/deb.debian.org/archive.debian.org/g' /etc/apt/sources.list; \
    sed -i 's/security.debian.org/archive.debian.org/g' /etc/apt/sources.list; \
    sed -i '/stretch-updates/d' /etc/apt/sources.list; \
    apt-get update; \
    apt-get upgrade -y --allow-unauthenticated; \
    chmod +x /usr/local/bin/install-php-extensions; \
    install-php-extensions \
        mcrypt \
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
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/* /usr/src/php.tar.xz

WORKDIR /var/www
COPY . /var/www
VOLUME /var/www

EXPOSE 80 8000

CMD ["bash"]
