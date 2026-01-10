---
number: 16926
title: "Allow reading `.env` files without opt-in"
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2025-12-02T10:42:30Z
updated_at: 2025-12-03T13:40:37Z
url: https://github.com/astral-sh/uv/issues/16926
synced_at: 2026-01-10T01:26:11Z
---

# Allow reading `.env` files without opt-in

---

_Issue opened by @zanieb on 2025-12-02 10:42_

This is a tracking issue for a sentiment expressed in

- #1384 
- https://github.com/astral-sh/uv/issues/8862

uv does not allow reading `.env` files without opt-in because we see reading `.env` files from checkouts of arbitrary projects as unsafe. People view this as introducing too much friction.

There are a few ways to persistently opt-in being discussed:

- https://github.com/astral-sh/uv/issues/16672
- https://github.com/astral-sh/uv/issues/9994
- https://github.com/astral-sh/uv/issues/14500

We may also allow use of `.env` files without persistent opt-in, but that'd be a breaking change and I don't see how we'd address the security concerns.

---

_Referenced in [astral-sh/uv#1384](../../astral-sh/uv/issues/1384.md) on 2025-12-02 10:42_

---

_Referenced in [astral-sh/uv#8862](../../astral-sh/uv/issues/8862.md) on 2025-12-02 10:43_

---

_Comment by @samypr100 on 2025-12-03 03:41_

Another alternative is to only allow these settings to be configurable only from a system or user level configuration file (similarly mentioned in https://github.com/astral-sh/uv/issues/9461#issuecomment-2509801588). This approach combined with the concept of "trusted" projects would provide a great deal of flexibility plus address most security concerns and attack vectors I can think of from my perspective.

---

_Comment by @birdhackor on 2025-12-03 07:47_

Trusted projects + UV_DEFAULT_ENV_FILE should be good.

---

_Comment by @kompre on 2025-12-03 08:35_

`uv` does make use of environment variables to modify its own behavior, and in particular I'm interested in `UV_PROJECT_ENVIRONMENT` that will inform `uv` of the path for the project virtual environment (if different from default value); the `--env-file` option is only available for `uv run` command, but `uv` needs to know about that for any command that do venv stuff (e.g. `uv sync`), otherwise it will default to `.venv/`.

My current workaround is to set the `UV_PROJECT_ENVIRONMENT` as settings in the .code-workspace, but this only works for vscode, and I need to remember to set the env variables each time I spawn a new terminal session (or use another tool that will do that). AI agents such claude code needs to be set up specifically too.

I kinda get around the limitation of `--env-file` not being available to all `uv `command by running something like this:

```
uv run --env-file .env -- uv [COMMAND]
```




---

_Referenced in [astral-sh/uv#16964](../../astral-sh/uv/issues/16964.md) on 2025-12-03 13:40_

---

_Comment by @zanieb on 2025-12-03 13:40_

@kompre that's a different request. We can track that in https://github.com/astral-sh/uv/issues/16964

---
