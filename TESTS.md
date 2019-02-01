# Tests

## Running Tests

`go test` is used for testing. A running PostgreSQL
server is required, with the ability to log in. The
database to connect to test with is "pqgotest," on
"localhost" but these can be overridden using [environment
variables](https://www.postgresql.org/docs/9.3/static/libpq-envars.html).

Example:

	PGHOST=/run/postgresql go test

## Benchmarks

A benchmark suite can be run as part of the tests:

	go test -bench .

## Example setup (Docker)

Run a postgres container:

```
docker run -p 5432:5432 postgres
```

Run tests:

```
PGHOST=localhost PGPORT=5432 PGUSER=postgres PGSSLMODE=disable PGDATABASE=postgres go test
```


## Running tests that require a read-only postgres

Run a writeable postgres instance on 5432 as well as an additional, read-only postgres container on port 54321

```
docker run -p 5432:5432 postgres
docker run -p 54321:5432 -d postgres -c 'default_transaction_read_only=true'
```

Run tests, including those that require a read-only postgres instance

```
TEST_READONLY=true go test
```