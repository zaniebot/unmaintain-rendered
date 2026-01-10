---
number: 7682
title: "`requires-python = \"~=3.10\"` uses docker container python 3.11 and not a managed python 3.10"
type: issue
state: closed
author: gabriel-vanzandycke
labels:
  - needs-mre
assignees: []
created_at: 2024-09-25T08:44:46Z
updated_at: 2024-10-23T00:02:28Z
url: https://github.com/astral-sh/uv/issues/7682
synced_at: 2026-01-10T01:24:18Z
---

# `requires-python = "~=3.10"` uses docker container python 3.11 and not a managed python 3.10

---

_Issue opened by @gabriel-vanzandycke on 2024-09-25 08:44_

I use a docker container bundled with python 3.11.9 but my app requires python ~=3.10. 
Running `uv sync` uses python system rather than the expected python version `~=3.10`.
When enforcing `==3.10`, uv downloads and uses a managed python version as expected.

**Dockerfile**
```dockerfile
FROM python:3.11.9-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:0.4.10 /uv /bin/uv
ENTRYPOINT /bin/bash
ADD . /root

RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-install-workspace > /tmp/uv_sync.log 2>&1
```

**pyprojects.toml**
```pyproject.toml
[project]
name = "app"
version = "1.6.0"
description = ""
requires-python = "~=3.10"
dependencies = [
    "jupyter>=1.0",
]

[tool.uv]
package = true
```

**With `requires-python = "~=3.10"` in pyproject.toml:**
Inspecting the beginning of uv sync output shows it is using python 3.11.9.
```
$ docker build -t app:latest -f Dockerfile .
$ docker run -it --rm app:latest
# head -1 /tmp/uv_sync.log
Using Python 3.11.9 interpreter at: /usr/local/bin/python3
```

**With `requires-python = "==3.10"` in pyproject.toml:**
When requiring `3.10` instead, inspecting the uv sync output shows it is using python 3.10 as expected.
```
$ docker build -t app:latest -f Dockerfile .
$ docker run -it --rm app:latest
root@d93a4e3c8011:/# head -1 /tmp/uv_sync.log
Using Python 3.10.0
```


---

_Renamed from "requires-python = "~=3.10" uses docker container python 3.11 and not a python 3.10" to "requires-python = "~=3.10" uses docker container python 3.11 and not a managed python 3.10" by @gabriel-vanzandycke on 2024-09-25 08:44_

---

_Renamed from "requires-python = "~=3.10" uses docker container python 3.11 and not a managed python 3.10" to "`requires-python = "~=3.10"` uses docker container python 3.11 and not a managed python 3.10" by @gabriel-vanzandycke on 2024-09-25 08:45_

---

_Comment by @weihenglim on 2024-09-25 09:14_

The version specifier `~=3.10` is equivalent to `>=3.10, ==3.*`, which matches `3.11`

You should specify `~=3.10.0` if you want to target Python `3.10.*`

---

_Closed by @gabriel-vanzandycke on 2024-09-25 09:26_

---

_Comment by @gabriel-vanzandycke on 2024-09-25 09:26_

Thanks. I didn't know that.

---

_Comment by @gabriel-vanzandycke on 2024-09-25 10:04_

I still have the problem with `~=3.10.0` though

---

_Reopened by @gabriel-vanzandycke on 2024-09-25 10:04_

---

_Comment by @gabriel-vanzandycke on 2024-09-25 10:06_

it's not easy to replicate my exact setup in a minimum working example, but when I use `~=3.10.0`, running
```
uv run jupyter notebook
```
and importing one of my dependency within a notebook fails with "module not found". It all seems like uv run is not loading/using the environment created previously.

---

_Comment by @weihenglim on 2024-09-25 10:15_

I'm not familiar with `jupyter` but you can try taking a look at https://github.com/astral-sh/uv/issues/7679#issuecomment-2373102667

---

_Comment by @charliermarsh on 2024-09-25 12:36_

If `~=3.10.0` is giving you `3.11.X`, that may be a bug.

---

_Comment by @charliermarsh on 2024-09-25 12:46_

I cannot produce that:
```
Using Python 3.11.7
error: The requested interpreter resolved to Python 3.11.7, which is incompatible with the project's Python requirement: `>=3.10.0, <3.11`
```

---

_Label `needs-mre` added by @charliermarsh on 2024-09-25 12:46_

---

_Referenced in [astral-sh/uv#7426](../../astral-sh/uv/issues/7426.md) on 2024-09-25 19:10_

---

_Comment by @charliermarsh on 2024-10-23 00:02_

Happy to reopen if we're able to find a reproduction.

---

_Closed by @charliermarsh on 2024-10-23 00:02_

---

_Referenced in [astral-sh/uv#14335](../../astral-sh/uv/pulls/14335.md) on 2025-07-01 21:24_

---
