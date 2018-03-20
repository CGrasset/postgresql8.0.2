Based on the work of: https://github.com/GuildEducationInc/docker-amazon-redshift

# Docker Amazon Redshift

This is an unofficial implementation of Amazon Redshift inside a Docker container. At this time, the container is merely PostgreSQL 8.0.2 running  on port 5432 inside the container. No other changes have been made to make it more closely match the behavior of Redshift.

There are two variants: one based on Debian Jessie and another based on Alpine 3.5. The Dockerfiles and entrypoint scripts are closely based on those used in the [official Postgres images](https://hub.docker.com/_/postgres/).

## How to use the image

### Running
`docker run -d --name my-postgresql802 cgrasset/postgresql8.0.2`

Port 5432 is set via an `EXPOSE` command so it should be available to linked containers. Optionally, you can map it to the host:

`docker run -d -p 5432:5432 --name my-postgresql802 cgrasset/postgresql8.0.2`

### Persisting Data

In order to have data persist between container runs, map the `PGDATA` directory to a volume. This variable is defaulted to `/var/lib/postgresql/data`:

`docker run -d -p 5432:5432 -v /path/on/host:/var/lib/postgresql/data --name my-postgresql802 cgrasset/postgresql8.0.2`

`initdb` is run on the `PGDATA` directory automatically on container start

### Setting a password

It is recommended to set a password for the default postgres user:

`docker run -d -p 5432:5432 -v /path/on/host:/var/lib/postgresql/data -e POSTGRES_PASSWORD=your_password --name my-postgresql802 cgrasset/postgresql8.0.2`

## Environment Variables

* `PGPORT` - port Postgres will run on. Default is `5432`.
* `PGDATA` - Data directory for Postgres. Default is `/var/lib/postgresql/data`.
* `POSTGRES_PASSWORD` - password for the database user. Default is no password.
* `POSTGRES_USER` - Sets a different user to run the database under. Default is `postgres`.
* `POSTGRES_DB` - Sets the name of the db that is created during the `initdb` run. Note the default `postgres` db will be created anyway.
* `POSTGRES_INITDB_ARGS` - Sets additional `initdb` args.
* `POSTGRES_INITDB_XLOGDIR` - Defines a different location for the Postgres transaction log
