```yaml
number: 7677
title: "Add a `uv-python` shim executable"
type: pull_request
state: open
author: zanieb
labels:
  - no-build
assignees: []
draft: true
base: main
head: zb/python-shim
created_at: 2024-09-24T22:31:28Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/7677
synced_at: 2026-01-12T16:07:56Z
```

# Add a `uv-python` shim executable

---

_@zanieb_

Adds a minimal `uv-python` binary to our distributions, which uses `uv python find` to determine which Python executable to use then invokes it with all the arguments.

Some notes:

- We call this `uv-python` instead of `python` so we don't replace `python` on the `PATH` when uv is installed.
- Respects virtual environments, but we might want to skip virtual environments by default? We have a flag to opt-out per invocation, at least.
- We copy (or link) this into the user's bin as `python` during `uv python install`. We'll probably want more ways to manage the shim?
- We add an environment variable to `Interpreter::query` calls to avoid recursive queries once a shim is installed
- Adds `--shim` / `--no-shim` flags to `uv python install`. By default, we only install a shim if a CPython variant is installed and preview is enabled.

Supports a few options, which can be used together:

- Version requests e.g. `uv-python +3.12 ...`.
- Only use a managed interpreter e.g. `uv-python +managed ...`.
- Ignore virtual environments e.g. `uv-python +system ...`.
- Enable verbose output e.g., `uv-python +v ...`

Unlike `uvx`, we use `which` to find the uv binary instead of only looking next to the file. This is because we need this to be copyable into the user's path. Unfortunately this makes it hard for us to guarantee that we are finding the correct version of uv if multiple are installed, but that seems like an edge-case. We could add some sort of `UV_REQUIRED_VERSION` variable or something that tells the uv binary to bail if it's not the right version. We prefer a binary that we find next to `uv-python` which could help alleviate this — I'll need to look at what happens with symlinks and such.

An alternative implementation is something like `uvx` where we add a `uv python run` command and then create aliases to it. That route is powerful because we can have advanced behaviors and rely on other crates without increasing the size of our installation. However, the constraints of this approach may be a good thing. 

Unfortunately we need to invoke uv to find the Python interpreter to use and uv queries Python interpreters so this probably adds some significant overhead to a `python` invocation. I'll benchmark this before moving out of draft.


---

_Label `no-build` added by @zanieb on 2024-09-25 17:14_

---

_Comment by @inoa-jboliveira on 2024-10-06 20:03_

Cool! would it be possible to call the executable `uvpy` so it behaves similarly to `py` on windows and is easier to type?

FYI you can do things like this on Windows:
```
py -3.10
```

It is something I miss when using mac

---

_Comment by @rtaycher on 2024-12-17 21:09_

Would be nice to run sync or at least have the option to run sync(with uv specific key in pyproject.toml). Could help teams maintain a consistent environment (especially if some users don't know as much about package management). 

---
