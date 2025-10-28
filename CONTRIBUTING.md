# Contributing

Thanks for contributing to this repo! This is a short guide to set you up for running Ballsdex in
a development environment, with some tips on the code structure.

## Setting up the environment

### Installing the dependencies

1. Get Python 3.13 and pip.
2. Run `python3 -m venv venv`.
3. Run `pip install -U -r requirements.txt`.

### Starting the bot

```bash
python3 -m ballsdex --dev --debug
```

You can do `python -m ballsdex -h` to see the available options.

### Starting the admin panel

```bash
cd admin_panel
python3 manage.py migrate
python3 manage.py collectstatic --no-input
uvicorn --reload --reload-include "*.html" admin_panel.asgi:application
```

You will be running the admin panel with additional debug tools. There is the django debug
toolbar to inspect SQL queries, loading times, template loading and other tools. You also get
pyinstrument, allowing you to profile a page by appending `?profile` at the end.

> [!TIP]
> `python3 manage.py` contains a lot of commands, feel free to explore them! To name a few:
>
> - `shell` launches a Python REPL ready to interact with models and database
> - `dbshell` will launch `psql` with the right settings for the database
> - `check` performs general system checks to ensure everything works
> - `createsuperuser` creates a superuser account
> - `showmigrations` shows the applied/missing migrations

> [!WARNING]
> Do not use `python3 manage.py runserver` to run the server, since the bot relies on async code.
> Django must be started with an ASGI server, not the default WSGI.

## Migrations

If you are modifying models definition, you need migrations to update the database schema.

First, synchronize your changes between `ballsdex/core/models.py` and
`admin_panel/bd_models/models.py`, they must be identical!

Then you can run `python3 manage.py makemigrations` to generate a migration file. Re-read its
contents to ensure there is only what you modified, and commit it.

You can read more about migrations
[here](https://docs.djangoproject.com/en/5.1/topics/migrations/), the engine is very extensive!

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

All rules are defined in `pyproject.toml`, meaning your editor will pick them up if you install
the right tools.