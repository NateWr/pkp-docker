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

Change the permissions on the local directories.

```
chmod -R <user>:www-data ./app
chmod -R <user>:www-data ./files
```

Open [http://localhost:8000](http://localhost:8000) in a browser to complete the installation.

## Notes

* Does not support `restful_urls` (mod rewrite).
