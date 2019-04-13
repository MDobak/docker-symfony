FROM mdobak/php-common-stack:7.3-fpm-alpine

ARG uid
ARG gid

# Default user
RUN    addgroup -g ${gid} -S app \
    && adduser -D -S -u ${uid} -g ${gid} -s /bin/bash app app \
# Common tools
    && apk --no-cache add \
        $PHPIZE_DEPS \
        curl \
        gnupg \
        binutils-gold \
        linux-headers \
        make \
        bash \
        sudo \
        zip \
# Composer
        zip \
        unzip \
        wget \
        curl \
        git \
# Zip
        libzip \
        libzip-dev \
        zlib-dev \
# PHP extensions
    && docker-php-ext-install zip \
# Composer permissions
    && chmod 755 /usr/local/bin/composer \
    && chown app:app $COMPOSER_HOME \
    && chown app:app $COMPOSER_HOME -R \
    && chmod 775 $COMPOSER_HOME \
    && chmod 775 $COMPOSER_HOME -R \
# PHP and workspace configuration
    && sed -i -E "s/^user *=.+$/user = app/" /usr/local/etc/php-fpm.d/www.conf \
    && sed -i -E "s/^group *=.+$/group = app/" /usr/local/etc/php-fpm.d/www.conf \
    && mkdir -p /app \
    && chown app /app \
    && chown -R app:app /app \
    && touch /usr/local/etc/php/php.ini \
    && docker-pcs-php-ext-config php --ini-name ../php.ini \
        log_errors=On \
        access_log=/dev/stdout \
        error_log=/dev/stderr \
        session.save_path=/tmp \
    && echo 'alias sf="php /app/bin/console"' >> /root/.bashrc \
    && echo 'alias sf="php /app/bin/console"' >> /home/app/.bashrc \
# Cleanup
    && chown app:app /app -R \
    && apk --no-cache --purge del \
        $PHPIZE_DEPS \
        gnupg \
        binutils-gold \
        linux-headers \
        make \
        *-dev \
    && rm -rf /var/cache/apk/* /tmp/*

WORKDIR /app
