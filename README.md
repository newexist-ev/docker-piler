# docker-piler
Mail Piler as a Docker container (unofficial)

## Installation
    sudo docker-compose up -d
    sudo docker-compose exec piler bash
    cd /piler
    make install
    make postinstall

Also, edit `/usr/local/etc/piler/piler.conf` and set `mysqlhost`. You can also set `syslog_recipients` to `1` for easier debugging.

Then, copy `/var/piler/www/config-site.php` to `/usr/local/etc/piler/config-site.php` and restart the container with `sudo docker-compose restart piler`.

## Docker Compose example
    version: "3"

    services:
      piler:
        build: https://github.com/alexhorn/docker-piler.git
        depends_on:
          - piler-db
        ports:
          - "80:80"
        volumes:
          - /var/lib/docker-piler/local:/usr/local/etc/piler
          - /var/lib/docker-piler/piler:/var/piler

      piler-db:
        image: mariadb:latest
        volumes:
          - /var/lib/docker-piler/mysql:/var/lib/mysql
        restart: unless-stopped
        environment:
          MYSQL_ROOT_PASSWORD: "changeme"
          MYSQL_DATABASE: "piler"
          MYSQL_USER: "piler"
          MYSQL_PASSWORD: "changeme"

## Credits
Contains [Mail Piler by Janus Suto](https://bitbucket.org/jsuto/piler/overview).

Also, I borrowed a config file from [nginx.com](https://www.nginx.com/resources/wiki/start/topics/examples/full/).
