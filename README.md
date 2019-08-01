# Docker Wordpress SQL

Simple docker for setup wordpress, db and phpmyadmin.

### How to use step by step

Start mariadb first

``` console
$ docker run -d --name db -v $PWD/db:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mariadb:10.4-bionic
```

then start wordpress and phpmyadmin

``` console
$ docker run -d --name wordpress -p 8080:80 -v $PWD/wordpress:/var/www/html --link db wordpress:5.2-php7.2
```

``` console
$ docker run -d --name phpmyadmin -p 8081:80 --link db phpmyadmin/phpmyadmin:4.8
```

### ... via [`docker stack deploy`](https://docs.docker.com/engine/reference/commandline/stack_deploy/) or [`docker-compose`](https://github.com/docker/compose)

Example [`stack.yml`](https://github.com/Pipekung/docker-wordpress-sql/blob/master/stack.yml)

``` yml
version: '3.1'

services:

  wordpress:
    image: wordpress:5.2-php7.2
    ports:
      - 8080:80
    volumes:
      - $PWD/wordpress:/var/www/html
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: root
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:4.8
    ports:
      - 8081:80
    depends_on:
      - db

  db:
    image: mariadb:10.4-bionic
    ports:
      - 3306:3306
    volumes:
      - $PWD/db:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root

```

Run

``` console
$ docker stack deploy -c stack.yml wordpress
```

or

``` console
$ docker-compose -f stack.yml up
```

and visit

- [`http://127.0.0.1:8080 - wordpress`](http://127.0.0.1:8080)
- [`http://127.0.0.1:8081 - phpmyadmin`](http://127.0.0.1:8081)
