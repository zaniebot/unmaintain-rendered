```yaml
number: 15739
title: "`uv run` discards free-threaded (3.13t) environments and recreates non-free-threaded ones"
type: issue
state: closed
author: Ravencentric
labels:
  - bug
assignees: []
created_at: 2025-09-08T17:33:11Z
updated_at: 2025-10-07T16:24:06Z
url: https://github.com/astral-sh/uv/issues/15739
synced_at: 2026-01-10T03:23:54Z
```

# `uv run` discards free-threaded (3.13t) environments and recreates non-free-threaded ones

---

_Issue opened by @Ravencentric on 2025-09-08 17:33_

I’ve been working on adding free-threaded support to my libraries. Like most libraries, mine support a wide range of Python versions (e.g., `requires-python = ">=3.10"`).

My first step was running `uv sync --python 3.13t` and then testing my code with `uv run ...`. To my surprise, `uv run` discarded the free-threaded environment and recreated a non-free-threaded one.

The only workarounds I’ve found are:
* Setting `UV_PYTHON=3.13t`. This applies globally, not just to the current project, which makes it unsuitable if I am juggling multiple projects.
* Adding `--python 3.13t` to every `uv run` invocation. This is repetitive and easy to forget.
* Activating the environment directly and skipping `uv run`. This forces me to think about venvs, which defeats the convenience of `uv run`.


# Minimal Reproducible Example

```console
❯ docker run --rm -it ghcr.io/astral-sh/uv:0.8.15-python3.13-bookworm /bin/sh
# uv init foobar --package --python ">=3.10"
Initialized project `foobar` at `/home/foobar`
# cd foobar
# uv sync --python 3.13t   # I want to use a free-threaded build
Using CPython 3.13.7
Creating virtual environment at: .venv
Resolved 1 package in 29ms
      Built foobar @ file:///home/foobar
Prepared 1 package in 21ms
Installed 1 package in 3ms
 + foobar==0.1.0 (from file:///home/foobar)
# uv run foobar   # uv run discards the free-threaded venv and rebuilds with a non-free-threaded interpreter
Using CPython 3.13.7 interpreter at: /usr/local/bin/python3.13
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Installed 1 package in 11ms
Hello from foobar!
# uv run python -c 'import sys; print(sys._is_gil_enabled())'
True
```


### Platform

Windows 11, Debian Bookworm

### Version

0.8.15

### Python version

Python 3.13t

---

_Label `bug` added by @Ravencentric on 2025-09-08 17:33_

---

_Assigned to @zanieb by @zanieb on 2025-09-08 17:35_

---

_Comment by @zanieb on 2025-09-16 23:06_

I think this is a duplicate of #12445

Thanks for the MRE!

---

_Closed by @zanieb on 2025-10-07 16:24_

---
