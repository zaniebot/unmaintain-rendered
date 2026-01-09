---
number: 7249
title: "`uv.lock` generated with `extra` as a string, but `uv sync` expects a sequence"
type: issue
state: closed
author: StrawIII
labels: []
assignees: []
created_at: 2024-09-10T09:48:31Z
updated_at: 2024-09-10T12:44:46Z
url: https://github.com/astral-sh/uv/issues/7249
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv.lock` generated with `extra` as a string, but `uv sync` expects a sequence

---

_Issue opened by @StrawIII on 2024-09-10 09:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Building an image from `Dockerfile` fails with an error due to `uv sync` expecting `extra` in `uv.lock` to be a sequence and not a string. After changing the line from
```
    { name = "pydantic", extra = "email" },
```
to
```
    { name = "pydantic", extra = ["email"] },
```
`uv sync` works.

* uv command: uv sync --frozen --no-cache
* uv platform: gnu linux
* uv version: 0.4.8

```dockerfile
FROM python:3.10

WORKDIR /app

COPY --from=ghcr.io/astral-sh/uv:0.4.8 /uv /bin/uv

COPY . .

RUN uv sync --frozen --no-cache

CMD ["/app/.venv/bin/uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--proxy-headers"]
```

```shell
> [backend stage-0 6/7] RUN uv sync --frozen --no-cache:
0.601 Using Python 3.10.15 interpreter at: /usr/local/bin/python3
0.601 Creating virtualenv at: .venv
0.611 Failed to parse lockfile; ignoring locked requirements: TOML parse error at line 619, column 34
0.611     |
0.611 619 |     { name = "pydantic", extra = "email" },
0.611     |                                  ^^^^^^^
0.611 invalid type: string "email", expected a sequence
0.612 error: Unable to find lockfile at `uv.lock`. To create a lockfile, run `uv lock` or `uv sync`.
------
failed to solve: process "/bin/sh -c uv sync --frozen --no-cache" did not complete successfully: exit code: 2
```

Part of the `uv.lock`
```toml
[[distribution]]
name = "project"
version = "0.1.0"
source = { editable = "." }
dependencies = [
    { name = "alembic" },
    { name = "authlib" },
    { name = "bcrypt" },
    { name = "boto3" },
    { name = "fastapi" },
    { name = "jinja2" },
    { name = "loguru" },
    { name = "psycopg2-binary" },
    { name = "pydantic" },
    { name = "pydantic", extra = "email" },
    { name = "pydantic-settings" },
    { name = "python-multipart" },
    { name = "sqlalchemy" },
    { name = "uvicorn" },
]
```



---

_Comment by @charliermarsh on 2024-09-10 12:02_

Thanks. Was the lockfile generated with the same uv version? Will take a look.

---

_Comment by @StrawIII on 2024-09-10 12:44_

I did some digging and found that I have somehow more than one uv installed. When using the latest 0.4.8 it works as intended (extra is a sequence, not a string).

---

_Closed by @StrawIII on 2024-09-10 12:44_

---
