# nginx-4-wordpress-fpm

`nginx-4-wordpress-fpm` docker images complements `wordpress:*-fpm` [official images](https://hub.docker.com/_/wordpress/). Apache alternatives serves http request. In order to allow PHP-fpm serve http request a webserver is needed (nginx).

This image uses `PHP_FPM_SOCK` environment variable to customize the location of the socket (usually port 9000 of php-fpm container).

Following, a `docker-compose.yml` that prepares a ready to use wordpress installation.

```
version: '2.0'

services:
  mysql:
    image: 'mariadb'
    environment:
      - MYSQL_ROOT_PASSWORD=secret

  php-fpm:
    image: wordpress:php7.1-fpm-alpine
    environment:
      - WORDPRESS_DB_USER=root
      - WORDPRESS_DB_PASSWORD=secret
    volumes:
      - './data/html:/var/www/html'

  web:
    image: packeteer/nginx-4-wordpress-fpm
    environment:
      - PHP_FPM_SOCK=php-fpm:9000
    ports:
      - 8080:80
    volumes:
      - './data/html:/var/www/html'
```
