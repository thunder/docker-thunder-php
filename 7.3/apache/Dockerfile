# hadolint ignore=DL3007
FROM php:7.3-apache-stretch

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install required libraries and packages for running Drupal.
RUN set -xe; \
    \
    apt-get update; \
    \
    apt-get install -y --no-install-recommends \
        git \
        libfreetype6-dev \
        libjpeg-dev \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        sudo \
        gnupg \
        apt-transport-https \
        netcat \
        unzip \
        mysql-client \
    ; \
    \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr \
        --with-jpeg-dir=/usr \
        --with-png-dir=/usr \
    ; \
    \
    docker-php-ext-install \
        gd \
        opcache \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        zip \
    ; \
    \
    apt list --installed | grep -o '.*-dev' | xargs apt-get purge -y; \
    \
    apt-get clean; \
    \
    rm -rf /var/lib/apt/lists/*; \
    \
    /usr/sbin/a2enmod rewrite;

# Setup PHP Cli OpCache
RUN { \
        echo 'opcache.enable=1'; \
        echo 'opcache.enable_cli=1'; \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=10000'; \
		echo 'opcache.validate_timestamps=0'; \
		echo 'opcache.fast_shutdown=1'; \
	} >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini;

# Create required user - add to sudoers so that the thunder user can run Apache.
RUN set -xe; \
    \
    adduser --gecos "" --disabled-password thunder; \
    \
    usermod --append --groups sudo thunder; \
    \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers ;

# Install composer
RUN set -xe; \
    \
    curl https://getcomposer.org/installer --output composer-setup.php; \
    \
    php composer-setup.php; \
    \
    rm -rf composer-setup.php; \
    \
    mv ./composer.phar /usr/local/bin/composer; \
    \
    chmod +x /usr/local/bin/composer;

ENTRYPOINT ["docker-php-entrypoint"]
CMD ["php", "-a"]
