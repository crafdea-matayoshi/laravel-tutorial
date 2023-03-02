FROM php:8.1.16-apache

WORKDIR .

RUN set -eux; \
        apt-get update; \
        apt-get install -y --no-install-recommends \
            unzip \
            vim \
            zip \
        ; \
        rm -rf /var/lib/apt/lists/*

COPY --from=composer /usr/bin/composer /usr/bin/composer

# Use the default production configuration /usr/local/etc/php
# RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" 本番開発用ini
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

ENV APACHE_DOCUMENT_ROOT /src/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

WORKDIR /src