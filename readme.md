# Docker for local development of PKP software

Build the docker image.

```
docker build -t pkp-<app>-<version> .
```

Update the app and version in `compose.yaml`.

```
name: <app>-<version>

services:
  pkp:
    image: pkp-<app>-<version>
```

Check out the OJS/OMP/OPS softare to a local `/app` directory.

```
git clone <url> app
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

Change permissions on the local directories that the Docker container will access.

```
chmod -R <user>:www-data ./app
chmod -R <user>:www-data ./config
chmod -R <user>:www-data ./files
```

Open [http://localhost:8000](http://localhost:8000) in a browser to complete the installation. Be sure to use `db` (from `compose.yaml`) as the database hostname.

## Notes

* Does not support `restful_urls` (mod rewrite).
