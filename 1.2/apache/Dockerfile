FROM php:7-apache

ENV GRAV_VERSION 1.2.4
ENV GRAV_DOWNLOAD_SHA256 c7cc861bc4838f3a5880d85791277c67b28dfbe8676e6e5d8fe323a960c6eb82

RUN mkdir -p /usr/src/grav \
    && curl -fSL -o /usr/src/grav/grav-admin.zip \
       "https://github.com/getgrav/grav/releases/download/$GRAV_VERSION/grav-admin-v$GRAV_VERSION.zip" \
    && echo "$GRAV_DOWNLOAD_SHA256  /usr/src/grav/grav-admin.zip" | sha256sum -c -

RUN set -xe \
    && buildDeps=" \
          unzip \
          libpng12-dev \
    " \
    && apt-get update && apt-get install -y libpng12-0 $buildDeps --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install gd zip \
    && ln -s /usr/src/grav /usr/src/grav/grav-admin \
    && unzip /usr/src/grav/grav-admin.zip -d /usr/src/grav \
    && rm /usr/src/grav/grav-admin.zip /usr/src/grav/grav-admin \
    && chown -R www-data:www-data /usr/src/grav \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $buildDeps

RUN a2enmod rewrite

COPY ./docker-entrypoint.sh /entrypoint.sh

ENTRYPOINT [ "/bin/sh", "/entrypoint.sh" ]
CMD [ "apache2-foreground" ]
