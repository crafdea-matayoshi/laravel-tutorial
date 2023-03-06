FROM php:8.1.16-apache

WORKDIR .

## パッケージインストール
RUN set -eux; \
        apt-get update; \
        apt-get install -y --no-install-recommends \
            unzip \
            vim \
            zip \
        ; \
        rm -rf /var/lib/apt/lists/* && \
        docker-php-ext-install pdo_mysql

RUN curl -sL https://deb.nodesource.com/setup_18.x |bash - && \
    apt-get install -y nodejs

COPY --from=composer /usr/bin/composer /usr/bin/composer

# Use the default production configuration /usr/local/etc/php
# RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" 本番開発用ini
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

## Apache configure
ENV APACHE_DOCUMENT_ROOT /src/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
RUN sed -i -e '/<Directory ${APACHE_DOCUMENT_ROOT}>/,/<\/Directory>/s/None/All/' /etc/apache2/apache2.conf
RUN ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load

WORKDIR /src