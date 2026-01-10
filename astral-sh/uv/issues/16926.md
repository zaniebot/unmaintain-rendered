```yaml
number: 16926
title: "Allow reading `.env` files without opt-in"
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2025-12-02T10:42:30Z
updated_at: 2025-12-12T19:33:43Z
url: https://github.com/astral-sh/uv/issues/16926
synced_at: 2026-01-10T03:11:35Z
```

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

_Comment by @zanieb on 2025-12-03 13:40_

@kompre that's a different request. We can track that in https://github.com/astral-sh/uv/issues/16964

---

_Comment by @chrisrodrigue on 2025-12-11 21:12_

Users need an `env-file` list setting in `pyproject.toml` so that they don't need to pass `--env-file` with every command invocation. 

Setting environment variables like `UV_ENV_FILE` via the OS/terminal defeats the common purpose of env files, which is to avoid setting system/user environment variables and introducing (often undocumented) state outside of the project.

---

_Comment by @AmanDevelops on 2025-12-12 19:33_

I’m currently using `poethepoet` as a task runner to work around this friction, and I think it’s a good concrete example of the kind of “persistent opt‑in” people are asking for.

My setup looks like this:

```toml
[tool.poe]
envfile = ".env"

[tool.poe.tasks]
dev = "fastapi dev app.py"
```

Then I run the server with:

```bash
uv run poe dev
```

This gives me:

- A single, short dev command (`poe dev`) that everyone on the team can use.
- Automatic `.env` loading for dev tasks, without having to remember `--env-file` every time.
- The opt‑in is explicit and versioned in `pyproject.toml`, so it’s clear that this project expects a `.env` file.

From a user perspective, this is exactly the kind of “project‑level opt‑in” that would be ideal in uv itself (e.g. a `[tool.uv]` setting that says “load `.env` for this project”). Right now, needing an extra task runner just to get this behavior feels like overkill, especially when uv already supports `--env-file` and `UV_ENV_FILE`.

I’d love to see uv support a similar persistent opt‑in mechanism (via config or a project‑level flag) so that teams can get the convenience of automatic `.env` loading without losing the security guarantees of explicit opt‑in.



---
