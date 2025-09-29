# WordPress with MariaDB and dump

This example defines a wordpress container architecture.

The architecture is prepared in Dockerfiles where the MariaDB consumes a dump
and the Apache gets supplied a Wordpress folder structure in the document root.

It is based on [WordPress image page](https://hub.docker.com/_/wordpress).

When deploying this setup, docker compose maps the WordPress container port 9875 to
port 9875 of the host as specified in the compose file.

## Prerequisites

This example uses our [Puppeteer Chrome Container](https://hub.docker.com/r/greencoding/puppeteer-chrome) and will download
it on the first measurement if you did not already pull it.

If you do want to alter this container you can also build it yourself from [Puppeteer Chrome Container](https://github.com/green-coding-berlin/example-applications/tree/main/puppeteer-chrome).
Only be sure to update the `usage_scenario.yml` with the local image identifier.

## Deploy with docker compose

``` bash
docker compose up -d
```

## Set hostnames for local debugging

Please set in `/etc/hosts` the following entry:
`127.0.0.1 gcb-wordpress-apache`

## Expected result

Check containers are running and the port mapping:

``` bash
$ docker compose ps -a
NAME                    IMAGE                   COMMAND                  SERVICE                CREATED              STATUS              PORTS
gcb-wordpress-apache    gcb_wordpress_apache    "docker-entrypoint.s…"   gcb-wordpress-apache   About a minute ago   Up 4 seconds        80/tcp, 0.0.0.0:9875->9875/tcp, :::9875->9875/tcp
gcb-wordpress-mariadb   gcb_wordpress_mariadb   "docker-entrypoint.s…"   gcb-wordpress-mariadb   7 minutes ago        Up 7 seconds        3306/tcp
```

Navigate to `http://gcb-wordpress-apache:9875` in your web browser to access WordPress.

Stop and remove the containers

``` bash
docker compose down
```

Once you are finished testing and want to remove all WordPress data you have to remove the image and delete the layer cache since we copy the data in:

``` bash
docker compose down -v
docker rmi -f gcb_wordpress_mariadb
docker system prune --volumes
```

## Peculiarities

The MariaDB database takes a long time to boot.

Therefore a `sleep 20` is in the `setup-commands` of the `usage_scenario.yml` so that Puppeteer will not  
get a database connection error from Wordpress.

## Changing the data in the admin

Go to http://gcb-wordpress-apache:9875/wp-admin/ and use:
- Username: arne
- Password: arne

## Running the measurement

To check how to run the measurements check out our [Documentation](https://docs.green-coding.berlin)

## OpenEnergyBadge
These badges show the energy cost for running this code on a single machine.

- <a href="https://metrics.green-coding.io/stats.html?id=a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad">
                    <img src="https://api.green-coding.io/v1/badge/single/a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad?metric=psu_carbon_ac_mcp_machine" loading="lazy">
                </a>
- <a href="https://metrics.green-coding.io/stats.html?id=a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad">
                    <img src="https://api.green-coding.io/v1/badge/single/a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad?metric=psu_energy_ac_mcp_machine" loading="lazy">
                </a>
- <a href="https://metrics.green-coding.io/stats.html?id=a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad">
                    <img src="https://api.green-coding.io/v1/badge/single/a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad?metric=embodied_carbon_share_machine" loading="lazy">
                </a>
- <a href="https://metrics.green-coding.io/stats.html?id=a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad">
                    <img src="https://api.green-coding.io/v1/badge/single/a6bd5efd-07ed-45c2-b3d9-7ec6b9c6e1ad?metric=network_carbon_formula_global" loading="lazy">
                </a>



