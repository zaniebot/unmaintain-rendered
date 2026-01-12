```yaml
number: 11206
title: "`uv sync` refers to system Python as a virtual environment when using `UV_PROJECT_ENVIRONMENT`"
type: issue
state: open
author: edmorley
labels:
  - bug
  - error messages
assignees: []
created_at: 2025-02-03T23:27:13Z
updated_at: 2025-02-04T00:11:45Z
url: https://github.com/astral-sh/uv/issues/11206
synced_at: 2026-01-12T16:00:30Z
```

# `uv sync` refers to system Python as a virtual environment when using `UV_PROJECT_ENVIRONMENT`

---

_@edmorley_

### Summary

When using `UV_PROJECT_ENVIRONMENT` and it points at a system Python installation, the verbose/debug logs refer to it as a virtual environment, when it's not. eg:

```Dockerfile
FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

ENV UV_PROJECT_ENVIRONMENT=/usr/local

WORKDIR /testcase
RUN uv init
RUN uv sync --verbose
```

```
$ docker build . --progress plain --no-cache
...
#10 [stage-0 5/5] RUN uv sync --verbose
#10 0.123 DEBUG uv 0.5.26
#10 0.125 DEBUG Found project root: `/testcase`
#10 0.125 DEBUG No workspace root found, using project root
#10 0.125 DEBUG Reading Python requests from version file at `/testcase/.python-version`
#10 0.125 DEBUG Using Python request `3.12` from version file at `.python-version`
#10 0.125 DEBUG Checking for Python environment at `/usr/local`
#10 0.221 DEBUG The virtual environment's Python version satisfies `3.12`
#10 0.222 DEBUG Using request timeout of 30s
#10 0.222 DEBUG Found static `pyproject.toml` for: testcase @ file:///testcase
#10 0.223 DEBUG No workspace root found, using project root
#10 0.224 DEBUG Solving with installed Python version: 3.12.8
#10 0.224 DEBUG Solving with target Python version: >=3.12
#10 0.225 DEBUG Adding direct dependency: testcase*
#10 0.225 DEBUG Searching for a compatible version of testcase @ file:///testcase (*)
#10 0.225 DEBUG Tried 1 versions: testcase 1
#10 0.225 DEBUG all marker environments resolution took 0.001s
#10 0.225 Resolved 1 package in 4ms
#10 0.226 DEBUG Using request timeout of 30s
#10 0.226 DEBUG Preserving seed package: pip==24.3.1
#10 0.226 Audited in 0.08ms
#10 DONE 0.2s
```

For example:
`The virtual environment's Python version satisfies 3.12`

This debug output originates from:
https://github.com/astral-sh/uv/blob/73e9928d406e9ea45eea7f69fb9ec578f38b4657/crates/uv/src/commands/project/mod.rs#L617-L634


The virtual environment references also happen in some error messages, eg:

```Dockerfile
FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

ENV UV_PROJECT_ENVIRONMENT=/usr/local

WORKDIR /testcase
RUN uv init --python 3.13
RUN uv sync --verbose
```

```
$ docker build . --progress plain --no-cache
...
#10 [stage-0 5/5] RUN uv sync --verbose
#10 0.120 DEBUG uv 0.5.27
#10 0.122 DEBUG Found project root: `/testcase`
#10 0.122 DEBUG No workspace root found, using project root
#10 0.122 DEBUG Reading Python requests from version file at `/testcase/.python-version`
#10 0.122 DEBUG Using Python request `3.13` from version file at `.python-version`
#10 0.122 DEBUG Checking for Python environment at `/usr/local`
#10 0.218 DEBUG The virtual environment's Python version does not satisfy `3.13`
#10 0.218 DEBUG Searching for Python 3.13 in managed installations or search path
#10 0.218 DEBUG Searching for managed installations at `/root/.local/share/uv/python`
#10 0.219 DEBUG Found `cpython-3.12.8-linux-aarch64-gnu` at `/usr/local/bin/python3` (search path)
#10 0.219 DEBUG Skipping interpreter at `/usr/local/bin/python3` from search path: does not satisfy request `3.13`
#10 0.318 DEBUG Found `cpython-3.12.8-linux-aarch64-gnu` at `/usr/local/bin/python` (search path)
#10 0.318 DEBUG Skipping interpreter at `/usr/local/bin/python` from search path: does not satisfy request `3.13`
#10 0.318 DEBUG Found `cpython-3.12.8-linux-aarch64-gnu` at `/usr/local/bin/python3` (search path)
#10 0.318 DEBUG Skipping interpreter at `/usr/local/bin/python3` from search path: does not satisfy request `3.13`
#10 0.318 DEBUG Found `cpython-3.12.8-linux-aarch64-gnu` at `/usr/local/bin/python` (search path)
#10 0.318 DEBUG Skipping interpreter at `/usr/local/bin/python` from search path: does not satisfy request `3.13`
#10 0.318 DEBUG Requested Python not found, checking for available download...
#10 0.319 DEBUG Acquired lock for `/root/.local/share/uv/python`
#10 0.319 DEBUG Using request timeout of 30s
#10 0.319 INFO Fetching requested Python...
#10 1.003 DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250115/cpython-3.13.1%2B20250115-aarch64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.temp/.tmpm4901a
#10 1.003 DEBUG Extracting cpython-3.13.1%2B20250115-aarch64-unknown-linux-gnu-install_only_stripped.tar.gz
#10 1.927 DEBUG Moving /root/.local/share/uv/python/.temp/.tmpm4901a/python to /root/.local/share/uv/python/cpython-3.13.1-linux-aarch64-gnu
#10 2.043 DEBUG Released lock at `/root/.local/share/uv/python/.lock`
#10 2.043 Using CPython 3.13.1
#10 2.043 error: Project virtual environment directory `/usr/local` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
```

...where the error message describes it as both a virtual environment and not at the same time :-)

> error: Project virtual environment directory `/usr/local` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment

The error message seems to originate from:
https://github.com/astral-sh/uv/blob/73e9928d406e9ea45eea7f69fb9ec578f38b4657/crates/uv/src/commands/project/mod.rs#L164

### Platform

Debian bookworm

### Version

0.5.27

### Python version

Python 3.12

---

_Label `bug` added by @edmorley on 2025-02-03 23:27_

---

_Comment by @zanieb on 2025-02-03 23:34_

We should just refer to "project environment" when feasible. Both in user-facing messaging and throughout our internals, we use "virtual environment" frequently for "a Python environment" whether its actually virtual or not.

---

_Label `error messages` added by @zanieb on 2025-02-03 23:34_

---

_Assigned to @zanieb by @zanieb on 2025-02-03 23:34_

---

_Comment by @edmorley on 2025-02-03 23:57_

Yeah agree :-)

Also, I think it would be helpful if this:

> cannot be used because it is not a compatible environment

...explained why it wasn't a compatible environment - since at the moment with non-verbose logs there's no way to tell.

(Let me know if you'd like this filed as a separate issue)

---

_Comment by @zanieb on 2025-02-04 00:01_

I can take a look at that too, but generally it means that it didn't fulfill your Python request.

---

_Comment by @edmorley on 2025-02-04 00:11_

> but generally it means that it didn't fulfill your Python request.

Yeah in that testcase the Python version was (intentionally) mismatched.

---
