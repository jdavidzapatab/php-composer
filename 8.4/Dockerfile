# Builder stage: Use official PHP image to compile extensions
FROM php:8.4-cli AS builder

# Install-php-extensions script
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install PHP extensions
RUN set -eux; \
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
        sockets

# Final stage: Use Docker Hardened Image
FROM dhi.io/php:8.4-dev

LABEL maintainer="David Zapata <jdavid.zapatab@gmail.com>"

ENV DEBIAN_FRONTEND=noninteractive

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy extensions from builder
# PHP 8.4 extension dir: no-debug-non-zts-20240924
COPY --from=builder /usr/local/lib/php/extensions/no-debug-non-zts-20240924/ /opt/php-8.4/lib/php/extensions/no-debug-non-zts-20240924/
COPY --from=builder /usr/local/etc/php/conf.d/docker-php-ext-* /opt/php-8.4/etc/php/conf.d/

# Install runtime dependencies for the extensions and cleanup
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
        libfreetype6 \
        libjpeg62-turbo \
        libpng16-16t64 \
        libxpm4 \
        libwebp7 \
        libavif16 \
        libzip5 \
        libpq5 \
        libxml2 \
        libsnappy1v5 \
    ; \
    echo "deb http://deb.debian.org/debian sid main" > /etc/apt/sources.list.d/sid.list; \
    apt-get update; \
    apt-get install -y --no-install-recommends -t sid libnghttp2-14 tar libpng16-16t64; \
    rm -f /etc/apt/sources.list.d/sid.list; \
    apt-get update; \
    rm -f /var/lib/dpkg/status.d/libnghttp2-14 /var/lib/dpkg/status.d/tar /var/lib/dpkg/status.d/openssl; \
    rm -f /opt/php-8.4/etc/php/conf.d/docker-php-ext-sodium.ini; \
    rm -f /opt/php-8.4/etc/php/conf.d/docker-php-ext-opcache.ini; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/*

WORKDIR /var/www
COPY . /var/www
VOLUME /var/www

EXPOSE 80 8000

ENTRYPOINT []
CMD ["bash"]