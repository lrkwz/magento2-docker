# Docker container for Magento 2 (CentOS)

This container contains all-in-one the full stack needed to experiment (develop) with Magento2:

- CentOS 6.x
- PHP 5.6
- MySQL 5.6
- Nginx 6.x
- Varnish 4.x

Said so this is **not** the optimal deployment schema for production.

## How to use Docker container (currently beta1)

- Execute command:

```
$ docker pull magento/centos:v1
```

- Then add virtual host to *hosts* file

```
127.0.0.1 magento2.docker.loc
```

- Launch container

```
$ docker run --cap-add=ALL -p 127.0.0.1:80:80 -e VIRTUAL_HOST=magento2.docker.loc -i -t magento/centos:v1 /bin/bash
```

- After container is launched, start services

```
$ service php-fpm start && service nginx start && service mysqld start && service varnish start
```
- You can now browse to http://magento2.docker.loc and http://magento2.docker.loc/admin (login with admin/123123q)

- Exit from the container to stop all the services; all the session data (new orders, customer registration ...) are *lost*: database is reset to his initial state so you can play safely with configuration.



## How to build image (and update to latest Magento2)

- Clone repository

- Move (```cd magento2-docker```) into cloned directory

- Clone Magento2 from repository [magento/magento2](https://github.com/magento/magento2)

- Move (```cd magento2```) into magento2 directory

- Run composer

- Move back to docker directory (```cd ..```)

- In console execute command for building Docker image (may take **long** please be patient)

```
$ docker build -t <image-name>:<version> .
```

- After build is complete, add virtual host to *hosts* file

```
127.0.0.1 magento2.docker.loc
```

- Launch container

```
$ docker run --cap-add=ALL -p 127.0.0.1:80:80 -e VIRTUAL_HOST=magento2.docker.loc -i -t <image-name>:<version> /bin/bash
```

- After container is launched, start services

```
$ service php-fpm restart && service nginx restart && service mysqld restart && service varnish restart
$ service php-fpm status && service nginx status && service mysqld status && service varnish status

```
Repeat ```service start``` for each service not showing up correctly.

*This container is for testing use - not for production environment.*
