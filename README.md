# Thunder PHP Docker image

## Dependencies

This image requires MySQL server. You can run MySQL on host machine or inside docker.

## How to use this image

The basic pattern for starting a Thunder instance is:

`docker run --name thunder -p 8080:8080/tcp -e "DB_HOST=mysql-host" thunder-php:latest`

The container will require some time to install Thunder and after that it will start to serve on port 8080.

## Available environment variables

### Basic options

- `DB_HOST` - database host
- `DB_NAME` - database name
- `DB_USER` - database username
- `DB_PASS` - database password
- `DB_PORT` - database port
- `DB_DIVER` - database driver. Allowed options are `mysql` and `pgsql` **(not tested)**

### Thunder specific options

- `BRANCH_NAME` - Thunder distribution branch
- `THUNDER_PROJECT` - Thunder project branch
- `ADDITIONAL_PHP_PACKAGES` - Composer install will use provided packages add then to Thunder installation **(not tested)**

### How to build image

In base repository directory execute following command to build 7.3-cli variation of image.

`docker build -f 7.3/cli/Dockerfile .`
