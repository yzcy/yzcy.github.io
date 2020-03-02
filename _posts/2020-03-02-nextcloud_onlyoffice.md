---
layout: post
title:  Nextcloud Onlyoffice
date:   2020-3-01 21:28:58
categories: Linux
tags: Linux
---

# Nextcloud Onlyoffice

-------------------------------

Document Server and Nextcloud Docker installation will install the preconfigured version of [ONLYOFFICE Document Server](https://github.com/ONLYOFFICE/DocumentServer) connected to Nextcloud to your server running them in Docker containers.

## Requirements

- The latest version of Docker (can be downloaded here: https://docs.docker.com/engine/installation/)
- Docker compose (can be downloaded here: https://docs.docker.com/compose/install/)

## Installation

1. Get the latest version of this repository running the command:

```
git clone https://github.com/ONLYOFFICE/docker-onlyoffice-nextcloud
cd docker-onlyoffice-nextcloud
```

2. edit docker-compose.yaml to bellow

   ~~~shell
   version: '3'
   services:
     app:
       container_name: app-server
       image: nextcloud:fpm
       stdin_open: true
       tty: true
       restart: always
       expose:
         - '80'
         - '9000'
       volumes:
         - ./docker-onlyoffice-nextcloud_app_data:/var/www/html
     onlyoffice-document-server:
       container_name: onlyoffice-document-server
       image: onlyoffice/documentserver:latest
       stdin_open: true
       tty: true
       restart: always
       expose:
         - '80'
         - '443'
       volumes:
         - ./docker-onlyoffice-nextcloud_document_data:/var/www/onlyoffice/Data
         - ./docker-onlyoffice-nextcloud_document_log:/var/log/onlyoffice
     nginx:
       container_name: nginx-server
       image: nginx
       stdin_open: true
       tty: true
       restart: always
       ports:
         - 80:80
         - 443:443
       volumes:
         - ./nginx.conf:/etc/nginx/nginx.conf
         - ./docker-onlyoffice-nextcloud_app_data:/var/www/html
   ~~~

   

3. Run docker Compose

   Please note**: the action must be performed with **root** rights.

   ~~~shell
   docker-compose up -d
   ~~~

   

**Please note**: you might need to wait a couple of minutes when all the containers are up and running after the above command.

4. Now launch the browser and enter the webserver address. The Nextcloud  wizard webpage will be opened. Enter all the necessary data to complete  the wizard.

5. Go to the project folder and run the `set_configuration.sh` script:

   Please note**: the action must be performed with **root** rights.

   ```shell
   bash set_configuration.sh
   ```

   

6. you might get this error

   Error: cURL error 28: Operation timed out after 120000 milliseconds with 0 out of 981883 bytes received (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)

7. get onlyoffice

   ~~~shell
   git clone https://github.com/ONLYOFFICE/onlyoffice-nextcloud.git onlyoffice
   chown -R www-data:www-data onlyoffice
   cp -rp onlyonfice to docker-onlyoffice-nextcloud_app_data/app
   ~~~

8.  you better to restart docker-compose 

   ~~~shell
   docker-compose restart
   ~~~

9. open website http://x.x.x.x/index.php/settings/apps/customization

10. search onlyoffice then enable it 

## Summarize

------

1. git clone https://github.com/ONLYOFFICE/docker-onlyoffice-nextcloud
2. cd docker-onlyoffice-nextcloud
3. change docker-compose file
4. docker-compose up -d
5.  bash set_configuration.sh
6. git clone https://github.com/ONLYOFFICE/onlyoffice-nextcloud.git onlyoffice
7. chown -R www-data:www-data onlyoffice
8. cp -rp onlyonfice to docker-onlyoffice-nextcloud_app_data/apps
9. open website http://x.x.x.x/index.php/settings/apps/customization
10. search onlyoffice
11. enable it 

------

### Recommended articles

https://github.com/ONLYOFFICE/docker-onlyoffice-nextcloud