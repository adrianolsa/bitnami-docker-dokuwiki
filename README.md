# What is DokuWiki?

> DokuWiki is a simple to use and highly versatile Open Source wiki software that doesn't require a database. It is loved by users for its clean and readable syntax. The ease of maintenance, backup and integration makes it an administrator's favorite

https://www.dokuwiki.org/

# Prerequisites

To run this application you need Docker Engine 1.10.0. Docker Compose is recomended with a version 1.6.0 or later.

# How to use this image


### Run the application using Docker Compose

This is the recommended way to run Dokuwiki. You can use the following docker compose template:

```
version: '2'
services:
  application:
    image: 'bitnami/dokuwiki:latest'
    ports:
      - '80:80'
      - '443:443'
    volumes:
      - 'dokuwiki_data:/bitnami/dokuwiki'
      - 'apache_data:/bitnami/apache'
volumes:
  dokuwiki_data:
    driver: local
  apache_data:
    driver: local
```

### Run the application manually

If you want to run the application manually instead of using docker-compose, these are the basic steps you need to run:

1. Create a new network for the application :

  ```
  $ docker network create dokuwiki_network
  ```


2. Run the Dokuwiki container:

  ```
  $ docker run -d -p 80:80 --name dokuwiki --net=dokuwiki_network bitnami/dokuwiki
  ```

Then you can access your application at http://your-ip/

## Persisting your application


If you remove every container and volume all your data will be lost, and the next time you run the image the application will be reinitialized. To avoid this loss of data, you should mount a volume that will persist even after the container is removed. If you are using docker-compose your data will be persistent as long as you don't remove `mariadb_data` and `application_data` data volumes. If you have run the containers manually or you want to mount the folders with persistent data in your host follow the next steps:

> **Note!** If you have already started using your application, follow the steps on [backing](#backing-up-your-application) up to pull the data from your running container down to your host.

### Mount persistent folders in the host using docker-compose

This requires a sightly modification from the template previously shown:
```
version: '2'

services:
  application:
    image: 'bitnami/dokuwiki:latest'
    ports:
      - '80:80'
      - '443:443'
    volumes:
      - '/path/to/your/local/dokuwiki_data:/bitnami/dokuwiki'
      - '/path/to/your/local/apache_data:/bitnami/apache'
```

### Mount persistent folders manually

In this case you need to specify the directories to mount on the run command. The process is the same than the one previously shown:

1. If you haven't done this before, create a new network for the application :

  ```
  $ docker network create dokuwiki_network
  ```

2. Run the Dokuwiki container:

  ```
  $ docker run -d -p 80:80 --name dokuwiki -v /your/local/path/bitnami/dokuwiki:/bitnami/dokuwiki --network=dokuwiki_network bitnami/dokuwiki
  ```

# Upgrade this application

Bitnami provides up-to-date versions of MariaDB and Dokuwiki, including security patches, soon after they are made upstream. We recommend that you follow these steps to upgrade your container. We will cover here the upgrade of the Dokuwiki container. For the MariaDB upgrade see https://github.com/bitnami/bitnami-docker-mariadb/blob/master/README.md#upgrade-this-image

1. Get the updated images:

```
$ docker pull bitnami/dokuwiki:latest
```

2. Stop your container

 * For docker-compose: `$ docker-compose stop dokuwiki`
 * For manual execution: `$ docker stop dokuwiki`

3. (For non-compose execution only) Create a [backup](#backing-up-your-application) if you have not mounted the dokuwiki folder in the host.

4. Remove the currently running container

 * For docker-compose: `$ docker-compose rm -v dokuwiki`
 * For manual execution: `$ docker rm -v dokuwiki`

5. Run the new image

 * For docker-compose: `$ docker-compose start dokuwiki`
 * For manual execution ([mount](#mount-persistent-folders-manually) the directories if needed): `docker run --name dokuwiki bitnami/dokuwiki:latest`

# Configuration
## Environment variables
 When you start the dokuwiki image, you can adjust the configuration of the instance by passing one or more environment variables either on the docker-compose file or on the docker run command line. If you want to add a new environment variable:

 * For docker-compose add the variable name and value under the application section:
```
application:
  image: bitnami/dokuwiki:latest
  ports:
    - 80:80
  environment:
    - DOKUWIKI_PASSWORD=my_password
  volumes_from:
    - application_data
```

 * For manual execution add a `-e` option with each variable and value:

```
 $ docker run -d -e DOKUWIKI_PASSWORD=my_password -p 80:80 --name dokuwiki -v /your/local/path/bitnami/dokuwiki:/bitnami/dokuwiki --network=dokuwiki_network bitnami/dokuwiki
```

Available variables:

 - `DOKUWIKI_USERNAME`: Dokuwiki application SuperUser name. Default: **superuser**
 - `DOKUWIKI_FULLNAME`: Dokuwiki SuperUser Full Name. Default: **Full Name**
 - `DOKUWIKI_PASSWORD`: Dokuwiki application password. Default: **bitnami**
 - `DOKUWIKI_EMAIL`: Dokuwiki application email. Default: **user@example.com**
 - `DOKUWIKI_WIKINAME`: Dokuwiki wiki name. Default: **Bitnami DokuWiki**



# Backing up your application

To backup your application data follow these steps:

1. Stop the running container:

* For docker-compose: `$ docker-compose stop dokuwiki`
* For manual execution: `$ docker stop dokuwiki`

2. Copy the Dokuwiki data folder in the host:

```
$ docker cp /your/local/path/bitnami:/bitnami/dokuwiki
```

# Restoring a backup

To restore your application using backed up data simply mount the folder with Dokuwiki data in the container. See [persisting your application](#persisting-your-application) section for more info.

# Contributing

We'd love for you to contribute to this container. You can request new features by creating an
[issue](https://github.com/bitnami/bitnami-docker-dokuwiki/issues), or submit a
[pull request](https://github.com/bitnami/bitnami-docker-dokuwiki/pulls) with your contribution.

# Issues

If you encountered a problem running this container, you can file an
[issue](https://github.com/bitnami/bitnami-docker-dokuwiki/issues). For us to provide better support,
be sure to include the following information in your issue:

- Host OS and version
- Docker version (`docker version`)
- Output of `docker info`
- Version of this container (`echo $BITNAMI_APP_VERSION` inside the container)
- The command you used to run the container, and any relevant output you saw (masking any sensitive
information)

# License

Copyright 2016 Bitnami

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
