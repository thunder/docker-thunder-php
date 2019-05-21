# Base PHP docker image for Thunder CMS

## How to use this image

This image contains PHP libraries required for Thunder CMS, on top of Official PHP Docker image. So you can simply execute:

`docker run -it burda/thunder-php:latest`

and execute some PHP functions. For example:

`echo "Hello from Thunder PHP";`

### How to build image

In base repository directory execute following command to build 7.3-cli variation of image.

`docker build -f 7.3/cli/Dockerfile .`
