# elgg
[![Build Status](https://img.shields.io/travis/demyxco/elgg?style=flat)](https://travis-ci.org/demyxco/elgg)
[![Docker Pulls](https://img.shields.io/docker/pulls/demyx/elgg?style=flat&color=blue)](https://hub.docker.com/r/demyx/elgg)
[![Architecture](https://img.shields.io/badge/linux-amd64-important?style=flat&color=blue)](https://hub.docker.com/r/demyx/elgg)
[![Alpine](https://img.shields.io/badge/alpine-3.10.2-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/elgg)
[![NGINX](https://img.shields.io/badge/nginx-1.17.3-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/elgg)
[![PHP](https://img.shields.io/badge/php-7.3.9-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/elgg)
[![Elgg](https://img.shields.io/badge/elgg-3.1.2-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/elgg)
[![Buy Me A Coffee](https://img.shields.io/badge/buy_me_coffee-$5-informational?style=flat&color=blue)](https://www.buymeacoffee.com/VXqkQK5tb)

Elgg is an award-winning open source social networking engine that provides a robust framework on which to build all kinds of social environments, from a campus wide social network for your university, school or college or an internal collaborative platform for your organization through to a brand-building communications tool for your company and its clients. 

TITLE | DESCRIPTION
--- | ---
USER<br />GROUP | www-data (82)<br />www-data (82)
WORKDIR | /var/www/html
PORT | 80
TIMEZONE | America/Los_Angeles
PHP | /etc/php7/php.ini<br />/etc/php7/php-fpm.d/php-fpm.conf
NGINX | /etc/nginx/nginx.conf<br />/etc/nginx/cache<br />/etc/nginx/common<br />/etc/nginx/modules<br />

## Updates
[![Code Size](https://img.shields.io/github/languages/code-size/demyxco/elgg?style=flat&color=blue)](https://github.com/demyxco/elgg)
[![Repository Size](https://img.shields.io/github/repo-size/demyxco/elgg?style=flat&color=blue)](https://github.com/demyxco/elgg)
[![Watches](https://img.shields.io/github/watchers/demyxco/elgg?style=flat&color=blue)](https://github.com/demyxco/elgg)
[![Stars](https://img.shields.io/github/stars/demyxco/elgg?style=flat&color=blue)](https://github.com/demyxco/elgg)
[![Forks](https://img.shields.io/github/forks/demyxco/elgg?style=flat&color=blue)](https://github.com/demyxco/elgg)

* Auto built weekly on Sundays (America/Los_Angeles)
* Rolling release updates

## Elgg Container
ENVIRONMENT | VARIABLE
--- | ---
ELGG_DOMAIN | domain.tld 
ELGG_DBUSER | demyx_user 
ELGG_DBPASSWORD | demyx_password 
ELGG_DBNAME | demyx_db 
ELGG_DBHOST | elgg_db 
ELGG_SITENAME | demyx 
ELGG_SITEEMAIL | info@domain.tld 
ELGG_WWWROOT | http://domain.tld/ 
ELGG_DISPLAYNAME | demyx 
ELGG_EMAIL | info@domain.tld 
ELGG_USERNAME | demyx 
ELGG_PASSWORD | demyx_password 
TZ | America/Los_Angeles | America/Los_Angeles

## MariaDB Container
ENVIRONMENT | VARIABLE
--- | ---
MARIADB_BINLOG_FORMAT | mixed
MARIADB_CHARACTER_SET_SERVER | utf8
MARIADB_COLLATION_SERVER | utf8_general_ci
MARIADB_DATABASE | # Optional
MARIADB_DEFAULT_CHARACTER_SET | utf8
MARIADB_INNODB_BUFFER_POOL_SIZE | 16M
MARIADB_INNODB_DATA_FILE_PATH | ibdata1:10M:autoextend
MARIADB_INNODB_FLUSH_LOG_AT_TRX_COMMIT | 1
MARIADB_INNODB_LOCK_WAIT_TIMEOUT | 50
MARIADB_INNODB_LOG_BUFFER_SIZE | 8M
MARIADB_INNODB_LOG_FILE_SIZE | 5M
MARIADB_INNODB_USE_NATIVE_AIO | 1
MARIADB_KEY_BUFFER_SIZE | 16M
MARIADB_KEY_BUFFER_SIZE | 20M
MARIADB_LOG_BIN | mysql-bin
MARIADB_MAX_ALLOWED_PACKET | 16M
MARIADB_MAX_ALLOWED_PACKET | 1M
MARIADB_MAX_CONNECTIONS | 151
MARIADB_MYISAM_SORT_BUFFER_SIZE | 8M
MARIADB_NET_BUFFER_SIZE | 8K
MARIADB_PASSWORD | # Optional
MARIADB_READ_BUFFER | 2M
MARIADB_READ_BUFFER_SIZE | 256K
MARIADB_READ_RND_BUFFER_SIZE | 512K
MARIADB_ROOT_PASSWORD | # Required
MARIADB_SERVER_ID | 1
MARIADB_SORT_BUFFER_SIZE | 20M
MARIADB_SORT_BUFFER_SIZE | 512K
MARIADB_TABLE_OPEN_CACHE | 64
MARIADB_USERNAME | # Optional
MARIADB_WRITE_BUFFER | 2M

## Usage
This config requires no .toml for Traefik and is ready to go when running: 
`docker-compose up -d`. If you want SSL, just remove the comments and make sure you have acme.json chmod to 600 (`touch acme.json; chmod 600 acme.json`) before mounting.

```
version: "3.7"

services:
  traefik:
    image: traefik
    container_name: demyx_traefik
    restart: unless-stopped
    command: 
      - --api
      - --api.statistics.recenterrors=100
      - --docker
      - --docker.watch=true
      - --docker.exposedbydefault=false
      - "--entrypoints=Name:http Address::80"
      #- "--entrypoints=Name:https Address::443 TLS"
      - --defaultentrypoints=http
      #- --defaultentrypoints=http,https
      #- --acme
      #- --acme.email=info@domain.tld
      #- --acme.storage=/etc/traefik/acme.json
      #- --acme.entrypoint=https
      #- --acme.onhostrule=true
      #- --acme.httpchallenge.entrypoint=http
      - --logLevel=INFO
      - --accessLog.filePath=/etc/traefik/access.log
      - --traefikLog.filePath=/etc/traefik/traefik.log
    networks:
      - demyx
    ports:
      - 80:80
      #- 443:443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      #- ./acme.json:/etc/traefik/acme.json # chmod 600
    labels:
      - "traefik.enable=true"
      - "traefik.port=8080"
      - "traefik.frontend.rule=Host:traefik.domain.tld"
      #- "traefik.frontend.redirect.entryPoint=https"
      #- "traefik.frontend.auth.basic.users=${DEMYX_STACK_AUTH}"
      #- "traefik.frontend.headers.forceSTSHeader=true"
      #- "traefik.frontend.headers.STSSeconds=315360000"
      #- "traefik.frontend.headers.STSIncludeSubdomains=true"
      #- "traefik.frontend.headers.STSPreload=true"
  elgg_db:
    container_name: elgg_db
    image: demyx/mariadb
    restart: unless-stopped
    networks:
      - demyx
    volumes:
      - elgg_db:/var/lib/mysql
    environment:
      MARIADB_DATABASE: demyx_db
      MARIADB_USERNAME: demyx_user
      MARIADB_PASSWORD: demyx_password
      MARIADB_ROOT_PASSWORD: demyx_root_password
  elgg:
    container_name: elgg
    image: demyx/elgg
    restart: unless-stopped
    networks:
      - demyx
    volumes:
      - elgg:/var/www/html
    depends_on:
      - elgg_db
    environment:
      ELGG_DOMAIN: domain.tld
      ELGG_DBUSER: demyx_user
      ELGG_DBPASSWORD: demyx_password
      ELGG_DBNAME: demyx_db
      ELGG_DBHOST: elgg_db
      ELGG_SITENAME: demyx
      ELGG_SITEEMAIL: info@domain.tld
      ELGG_WWWROOT: http://domain.tld/
      ELGG_DISPLAYNAME: demyx
      ELGG_EMAIL: info@domain.tld
      ELGG_USERNAME: demyx
      ELGG_PASSWORD: demyx_password
      TZ: America/Los_Angeles
    labels:
      - "traefik.enable=true"
      - "traefik.port=80"
      - "traefik.frontend.rule=Host:domain.tld,www.domain.tld"
      #- "traefik.frontend.redirect.entryPoint=https"
      #- "traefik.frontend.auth.basic.users=${DEMYX_STACK_AUTH}"
      #- "traefik.frontend.headers.forceSTSHeader=true"
      #- "traefik.frontend.headers.STSSeconds=315360000"
      #- "traefik.frontend.headers.STSIncludeSubdomains=true"
      #- "traefik.frontend.headers.STSPreload=true"
volumes:
  elgg:
    name: elgg
  elgg_db:
    name: elgg_db
networks:
  demyx:
    name: demyx
```
