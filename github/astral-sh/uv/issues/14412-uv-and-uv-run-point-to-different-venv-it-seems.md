---
number: 14412
title: "\"uv\" and \"uv run\" point to different venv (it seems)"
type: issue
state: closed
author: GitRon
labels:
  - question
assignees: []
created_at: 2025-07-02T09:13:31Z
updated_at: 2025-07-02T09:37:12Z
url: https://github.com/astral-sh/uv/issues/14412
synced_at: 2026-01-07T13:12:18-06:00
---

# "uv" and "uv run" point to different venv (it seems)

---

_Issue opened by @GitRon on 2025-07-02 09:13_

### Question

Hi there!

I've experiencing the wildest thing. I want to run a Django management command via my uv venv, so I run

> uv run python manage.py migrate

This fails due to missing packages.

So I've checked the installed packages with

> uv run pip freeze

I get a small list of packages, MANY from my lockfile are missing.

When I run this, all packages are there:

> uv pip freeze

When I try to run my Django command via uv alone (`uv python manage.py migrate`), I get this error:

> error: unrecognized subcommand '.\manage.py'

I've created my uv venv via `uv sync`.

Any ideas what I'm doing wrong? I'm out of ideas 🙈 

Thx ❤ 

### Platform

Win11 Pro

### Version

v0.7.18

---

_Label `question` added by @GitRon on 2025-07-02 09:13_

---

_Comment by @GitRon on 2025-07-02 09:37_

I have no idea why I get different results but the solution for my underlying problem was really stupid. I forgot to install `psycopg[binary]` - using the binary extra 🙈 I'll leave this open, maybe it helps someone else, too.

---

_Closed by @GitRon on 2025-07-02 09:37_

---
