FROM php:7.3-cli-stretch

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG NVM_VERSION="v0.34.0"

ARG DOCKERFILE_DIR="7.3/cli"

# Install packages needed for additional repos packages
RUN set -xe; \
    \
    apt-get update; \
    \
    apt-get install -y --no-install-recommends \
        apt-transport-https \
        gnupg \
    ; \
    \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -; \
    \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list; \
    \
    apt-get update; \
    \
    apt-get install -y --no-install-recommends \
        git \
        imagemagick \
        jq \
        libjpeg-dev \
        libmagickwand-dev \
        libpng-dev \
        libpq-dev \
        libxml2-dev \
        libzip-dev \
        mysql-client \
        unzip \
        yarn \
    ; \
    \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
    ; \
    \
    docker-php-ext-install \
        gd \
        opcache \
        pdo_mysql \
        pdo_pgsql \
        zip \
    ; \
    \
    pecl install imagick; \
    \
    docker-php-ext-enable imagick; \
    \
    pecl clear-cache; \
    \
    apt-get purge -y --auto-remove \
        apt-transport-https \
        gnupg \
        libjpeg-dev \
        libmagickwand-dev \
        libpng-dev \
        libpq-dev \
        libxml2-dev \
        libzip-dev \
        mysql-client \
    ; \
    \
    apt-get clean; \
    \
    rm -rf /var/lib/apt/lists/*;

# Setup PHP Cli OpCache
RUN { \
        echo 'opcache.enable=1'; \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=60'; \
		echo 'opcache.fast_shutdown=1'; \
	} > /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini;

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

# Create required user
RUN set -xe; \
    \
    adduser --gecos "" --disabled-password thunder; \
    \
    usermod -aG sudo thunder;

RUN set -xe; \
    \
    su - thunder -c "curl -o- https://raw.githubusercontent.com/creationix/nvm/$NVM_VERSION/install.sh | bash"; \
    \
    echo -e "\nexport NVM_DIR=\"\$HOME/.nvm\"\n[ -s \"\$NVM_DIR/nvm.sh\" ] && \. \"\$NVM_DIR/nvm.sh\"\n" >> /home/thunder/.profile;

# Install latest stable version of NodeJS
RUN set -xe; \
    \
    su - thunder -c "nvm install node";

# Copy build and install script
COPY $DOCKERFILE_DIR/thunder-php-build /usr/local/bin/
COPY $DOCKERFILE_DIR/thunder-php-install /usr/local/bin/
COPY $DOCKERFILE_DIR/thunder-php-serve /usr/local/bin/
COPY $DOCKERFILE_DIR/thunder-php-run /usr/local/bin/

# Set executable
RUN set -xe; \
    \
    chmod +x /usr/local/bin/thunder-php-build; \
    \
    chmod +x /usr/local/bin/thunder-php-install; \
    \
    chmod +x /usr/local/bin/thunder-php-serve; \
    \
    chmod +x /usr/local/bin/thunder-php-run;

# Define all runtime environments
ENV DB_HOST="127.0.0.1"
ENV DB_NAME="thunder"
ENV DB_USER="thunder"
ENV DB_PASS="thunder"
ENV DB_PORT="3306"
ENV DB_DIVER="mysql"

# Thunder distribution branch
ENV BRANCH_NAME="8.x-3.x"

# Thunder project branch
ENV THUNDER_PROJECT="3.x"

# Additional PHP packages for composer install
ENV ADDITIONAL_PHP_PACKAGES=""

EXPOSE 8080/tcp
CMD ["thunder-php-run"]
