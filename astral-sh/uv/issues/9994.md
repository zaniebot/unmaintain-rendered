```yaml
number: 9994
title: Opt into .env files via settings
type: issue
state: open
author: butterlyn
labels:
  - configuration
assignees: []
created_at: 2024-12-18T11:36:40Z
updated_at: 2025-12-11T21:22:33Z
url: https://github.com/astral-sh/uv/issues/9994
synced_at: 2026-01-10T03:11:32Z
```

# Opt into .env files via settings

---

_Issue opened by @butterlyn on 2024-12-18 11:36_

Feature request to have a option in settings (i.e., `pyproject.toml` or `uv.toml`) for setting a value equivalent to `UV_ENV_FILE` for a given project.

Similar to how the environment variable [UV_INDEX_URL](https://docs.astral.sh/uv/configuration/environment/#uv_index_url) has a corresponding setting [index-url](https://docs.astral.sh/uv/reference/settings/#index-url).

Typically `.env` files are created on a project-by-project basis, so it makes sense to specify the location of the `.env` file in the settings of a given project in the relevant `pyproject.toml` or `uv.toml` file.

---

_Renamed from "Opt-into .env files via settings" to "Opt into .env files via settings" by @butterlyn on 2024-12-18 11:36_

---

_Label `configuration` added by @samypr100 on 2024-12-19 01:03_

---

_Comment by @kevinrenskers on 2025-02-24 19:22_

I was just about to create the same issue but then found this. And yeah 100% agreed!

You can run uv using `uv run --env-file .env ./manage.py runserver`, to automatically load a dotenv file as environment values (in this example while running Django). This is handy, but it requires that you pass in `--env-file .env` with every command. While I can change the `manage.py` script to automatically do this (change the first line to `#!/usr/bin/env -S uv run --env-file .env`), it doesn't help when you also run other command such as `uv run mypy .` which you now also have to run with `--env-file .env`. 

It would be very nice if this could be configured like this, inside of `pyproject.toml`:

```
[tool.uv]
env-file = "soundradix/.env"
```

---

_Comment by @chrisrodrigue on 2025-12-11 21:22_

I would suggest this setting being a list that uv evaluates in order of appearance to enable overriding or extending variables (similar to other strategies such as index resolution).

```toml
[tool.uv]
env-file = [".env/app1.env", ".env/app2.env", "..."]
```

---
