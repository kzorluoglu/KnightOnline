FROM php:8.2-apache

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gnupg2 \
        ca-certificates \
        curl \
        unzip \
        apt-transport-https \
        unixodbc-dev \
        g++ \
        make \
        libzip-dev \
    && mkdir -p /usr/share/keyrings \
    && curl -fsSL https://packages.microsoft.com/keys/microsoft.asc \
        | gpg --dearmor \
        > /usr/share/keyrings/microsoft-prod.gpg \
    && curl -fsSL https://packages.microsoft.com/config/debian/12/prod.list \
        > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update \
    && ACCEPT_EULA=Y apt-get install -y --no-install-recommends msodbcsql18 \
    && pecl install sqlsrv pdo_sqlsrv \
    && docker-php-ext-enable sqlsrv pdo_sqlsrv \
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

COPY --chown=www-data:www-data ./www.ini /usr/local/etc/php/conf.d/
