---
number: 15560
title: Lock files are inconsistent on Windows when using private repos explicitly
type: issue
state: closed
author: sakost
labels:
  - bug
assignees: []
created_at: 2025-08-27T18:43:51Z
updated_at: 2025-10-23T06:40:32Z
url: https://github.com/astral-sh/uv/issues/15560
synced_at: 2026-01-07T13:12:19-06:00
---

# Lock files are inconsistent on Windows when using private repos explicitly

---

_Issue opened by @sakost on 2025-08-27 18:43_

### Summary

I'm trying to use `uv` with a setting explicit private repo:
```
# pyproject.toml
# ...
[tool.uv.sources]
internal-package = { index = "private-repo" }

[[tool.uv.index]]
name = "private-repo"
url = "https://gitlab.example.com/api/v4/projects/xxxx/packages/pypi/simple/"
explicit = true
```
When I'm locking via `uv lock` and then trying to use this lock file within Dockerfile like this:
```Dockerfile
# Install the project's dependencies using the lockfile and settings
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --locked --no-install-project --no-dev

# Then, add the rest of the project source code and install it
# Installing separately from its dependencies allows optimal layer caching
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev
```
> this code was taken from an [official](https://github.com/astral-sh/uv-docker-example) example of uv+docker.

And while building this image with docker compose `docker compose --env-file .env up --watch` it throws an error:
```bash
0.234 Using CPython 3.12.9 interpreter at: /usr/local/bin/python3.12
0.234 Creating virtual environment at: .venv
1.658 Resolved 142 packages in 1.41s
1.662 The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
------
failed to solve: process "/bin/sh -c uv sync --locked --no-dev" did not complete successfully: exit code: 1
```

You can check a full reproducible example of such a problem in my repo: https://github.com/sakost/uv-docker-example-win (forked from official)

The diff between two `uv.lock` (one from Docker, one from system) is just an extra slash at the end of the public pypi repo: `https://pypi.org/simple` vs `https://pypi.org/simple/`. The `https://pypi.org/simple` is from the system. 

This issue can be related to this problem, but I'm not sure: https://github.com/astral-sh/uv/issues/15043

### Platform

Windows 11 x86_64

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

### Python version

Python 3.12.9

---

_Label `bug` added by @sakost on 2025-08-27 18:43_

---

_Comment by @sakost on 2025-08-27 18:45_

I'll try to fix it tomorrow, but I'm not sure that I'm capable of it, so any help with this would be awesome

---

_Comment by @sakost on 2025-08-28 19:01_

Didn't find anything :(
It seems that it is a difficult bug to fix

---

_Comment by @charliermarsh on 2025-08-29 00:01_

If you see:

> https://pypi.org/simple vs https://pypi.org/simple/

That typically means that you're setting `UV_INDEX=https://pypi.org/simple/` or something like that. You must be adding that configuration somewhere, since uv omits the trailing slash by default.

---

_Comment by @sakost on 2025-10-23 06:40_

You're right!
Thank you so much

It was not obvious to me
Maybe we should add some notes about it in the doc?

Closing the issue

---

_Closed by @sakost on 2025-10-23 06:40_

---
