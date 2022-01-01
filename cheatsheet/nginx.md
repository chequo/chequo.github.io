# Docker

#### Show all images

```cmd
docker images
```

#### Clear all Docker images and containers and start with clean slate

```cmd
docker system prune -a
```

#### Build docker image (with Docker compose)

```cmd
docker-compose -f docler-compose.yml build

docker build . (if have Dockerfile)
```

#### Launch docker image

```
docker-compose -f docler-compose.yml up
```

#### docker config file

```yml
version: '2.2'

services:
  web:
    build: .
    ports:
      - 9000:9000
    volumes:
      - .:/apps/
      - /apps/vendor

  nginx:
    build: ./nginx
    ports:
      - "80:80"
  
```

# App

```
root
|____Dockerfile

```

- Dockerfile

```Dockerfile
FROM php:5.6.30-fpm-alpine
RUN apk update &amp;&amp; apk add build-base
RUN apk add postgresql postgresql-dev \
  &amp;&amp; docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
  &amp;&amp; docker-php-ext-install pdo pdo_pgsql pgsql
RUN apk add zlib-dev git zip \
  &amp;&amp; docker-php-ext-install zip
RUN curl -sS https://getcomposer.org/installer | php \
        &amp;&amp; mv composer.phar /usr/local/bin/ \
        &amp;&amp; ln -s /usr/local/bin/composer.phar /usr/local/bin/composer
COPY . /app
WORKDIR /app
RUN composer install --prefer-source --no-interaction
ENV PATH="~/.composer/vendor/bin:./vendor/bin:${PATH}"
```

- docker-compose.yml

```yml
version: '2'
services:
  nginx:
    image: nginx:1.11.10-alpine
    ports:
      - 3000:80
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
  web:
    build: .
    ports:
      - 9000:9000
    volumes:
      - .:/app
      - /app/vendor
    depends_on:
      - postgres
    environment:
      DATABASE_URL: postgres://todoapp@postgres/todos
  postgres:
    image: postgres:9.6.2-alpine
    environment:
      POSTGRES_USER: todoapp
      POSTGRES_DB: todos
```

If you want to run it as a web server, use the following Dockerfile:

FROM php:7.2-apache
COPY . /var/www/html/
build it:

docker build -t my-php-app .
and run it:

docker run -p 8080:80 -d my-php-app
you will then have your PHP script runnnig on 8080.
