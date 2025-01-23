# Contributing

Thanks for contributing to this repo! This is a short guide to set you up for running Ballsdex in
a development environment, with some tips on the code structure.

## Setting up the environment

### PostgreSQL and Redis

Without docker, check how to install and setup PostgreSQL and Redis-server on your OS.
Export the appropriate environment variables as described in the
[README](README.md#without-docker).

### Installing the dependencies

1. Get Python 3.13 and pip.
2. Run `python -m venv venv`.
3. Run `pip install -U -r requirements.txt`.

### Starting the bot

```bash
python -m ballsdex --dev --debug
```

You can do `python -m ballsdex -h` to see the available options.

### Starting the admin panel

**Warning: You need to run migrations at least once before starting the admin
panel without the other components.** You can either run the bot once or do `aerich upgrade`.

```bash
uvicorn ballsdex.core.admin:_app --host 0.0.0.0 --reload
```

## Migrations

When modifying the Tortoise models, you need to create a migration file to reflect the changes
everywhere. For this, we're using [aerich](https://github.com/tortoise/aerich).

### Applying the changes from remote

When new migrations are available, you can either start the bot to run them automatically, or
execute the following command:

```sh
aerich upgrade
```

### Creating new migrations

If you modified the models, `aerich` can automatically generate a migration file.

**You need to make sure you have already ran previous migrations, and that your database
is not messy!** Aerich's behaviour can be odd if not in ideal conditions.

Execute the following command to generate migrations, and push the created files:

```sh
aerich migrate
```

## Coding style

The code is formatted by `black`, style verified by `flake8`, and static checked by `pyright`.
They can be setup as a pre-commit hook to make them run before committing files:

```sh
pre-commit install
```

You can also run them manually:

```sh
pre-commit run -a
```
