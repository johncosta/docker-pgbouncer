# docker-pgbouncer

Creates a container running `pgbouncer` and `stunnel`.

The traffic flow is:

 * Ingress on `:5432` to `stunnel`
 * Into `pgbouncer` on `:6000`
 * Back into `stunnel`, egressing to your defined `POSTGRES_URL`

This allows for encrypted Postgres traffic into and out of the container.

`stunnel` and `pgbouncer` are both managed by a `supervisord` process in the container.

## Usage

```
docker run --name pgbnc0 -e "POSTGRES_URL=..." docker-pgbouncer
```

## Required Environment Variables

`POSTGRES_URL` must be passed to the container in the format

```
POSTGRES_URL=postgres://user:password@host:port/database
```

## Constraints

This container only supports a single upstream database.

`stunnel` is using a self-signed cert. Postgres in `sslmode=require` is fine with this, but the `verify` modes will fail unless you replace it with a real cert.
