# Docker for local development of PKP software

> **This is **not for production** deployments.** You may be looking for the [official PKP docker container](https://github.com/pkp/containers).

## Usage

Copy the Dockerfile for your desired app and version. (Usually, the OJS file can be used for all apps.)

```
cp Dockerfile.example.ojs-350 Dockerfile
```

Build the docker image.

```
docker build -t pkp-ojs-350 .
```

Copy the example docker compose file.

```
cp compose.example.ojs-350.yaml
```

Update the app and version in `compose.yaml`.

```
name: ojs-350

services:
  pkp:
    image: pkp-ojs-350
```

Update the volumes in `compose.yaml`.

```
    volumes:
      - ./apps/ojs-350:/var/www/html
      - ./files/ojs-350:/var/www/files
```


Clone the OJS/OMP/OPS software to the local `/apps` directory.

```
git clone <url> apps/ojs-350
```

Follow PKP's installation instructions.

> [Install OJS for local development.](https://docs.pkp.sfu.ca/dev/documentation/en/getting-started)

Set the `base_url` in `config.inc.php`.

```
base_url = "http://localhost:8000"
```

Set the files directory in `config.inc.php`.

```
files_dir = /var/www/files
```

Create a files directory for this instance.

```
mkdir files/ojs-350
```

Change permissions on the local directories that the Docker container will access.

```
chmod -R <user>:www-data ./apps
chmod -R <user>:www-data ./config
chmod -R <user>:www-data ./files
```

Run the containers.

```
docker compose up -d
```

Open [http://localhost:8000](http://localhost:8000) in a browser to complete the installation. Be sure to use `db` (from `compose.yaml`) as the database hostname.

## Datasets

Follow these steps to use PKP's test data in an instance.

Clone the test data.

```
git clone git@github.com:pkp/datasets.git --depth 1
```

Copy the files directory and set permissions.

```
cp -r datasets/ojs/stable-3_5_0/mysql/files ./files/ojs-350
chown -R <user>:www-data ./files/ojs-350
```

Load the database into the container.

```
docker cp datasets/ojs/stable-3_5_0/mysql/database.sql ojs-350-db-1:database.sql
```

Login to mysql/mariadb.

```
docker exec -it ojs-330-db-1 /bin/sh
mysql -u ojs -p
```

Recreate the database.

```
drop database ojs;
create database ojs;
use ojs;
source database.sql;
```

## Plugins

Plugins and themes can be stored in a `./plugins` directory and accesed by multiple containers with a symlink.

Clone the plugin into `./plugins`.

```
git clone <url> plugins/<name>
```

Symlink the plugin in local files to the `/var/www/plugins/` directory in the container. (This volume is defined in the compose file.)

```
ln -s /var/www/plugins/<name> ./plugins/<name>
```

# Errors

View errors in the `config/error_log` file. (See `config/php.custom.ini` and `compose.yaml`.)

## Notes

* Does not support `restful_urls` (mod rewrite).
* Write issues with the `cache` and `files` dirs can cause fatal errors. Make sure `www-data` has write access.
