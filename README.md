# Docker Wordpress SQL

stack.yml

``` yml
version: '3.1'

services:

  wordpress:
    image: wordpress
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
    image: phpmyadmin/phpmyadmin
    ports:
      - 8081:80
    depends_on:
      - db

  db:
    image: mariadb:10.4-bionic
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

- [```http://127.0.0.1:8080 - wordpress```](http://127.0.0.1:8080)
- [```http://127.0.0.1:8081 - phpmyadmin```](http://127.0.0.1:8081)