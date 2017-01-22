# Developer Installation Guide

## Installing Dependencies

### PostgreSQL & Redis

Refer to the documentation of Python, PostgreSQL, Redis and Node.js on how to install
them in your environment. On macOS, you can use `brew` to install them. On Linux
you can use your package manager, although need to make sure it installs recent
enough versions.

### Python Packages

For development the minimum required packages to install are described in:

* requirements.txt
* requirements_dev.txt

You install them with pip:

```bash
pip install -r requirements.txt -r requirements_dev.txt
```

(We recommend installing them in a virtualenv)

### Node.js Packages

Install all packages with:

```bash
npm install
```

## Configuration

For development, in most cases the default configuration is enough. But if you need
to adjust the database configuration, mail settings or any [other setting](../setup/settings-environment-variables.md),
you do so with environment variables.

To avoid having to export those variables manually, you can use a `.env` file and
the `bin/run` helper script. By invoking any command with `bin/run` prefix, it will
load your environment variables from the `.env` file and then run your command. For
example:

```bash
bin/run ./manage.py check_settings
```

## Creating Database Tables

TBD

## Processes

The main Redash processes you have to run:

* Web server
* Celery worker(s) & scheduler

In development you will also run Webpack's dev server or watch utility.

Our recommendation:

* Web server: `bin/run ./manage.py runserver --debugger --reload`
* Celery: `./bin/run celery worker --app=redash.worker --beat -Qscheduled_queries,queries,celery -c2`
* Webpack dev server: `npm run start`

This will result in a Flask web server listening on port `5000`, Webpack dev server
on port `8080` and 2 Celery workers ready to run queries.

To open the app in your web browser, use Webpack's dev server -- `localhost:8080`,
which will auto reload and refresh whenever you make changes to the frontend code.

## Running Tests

Currently we currently have tests only for the backend code. To run them invoke:

```bash
make test
```