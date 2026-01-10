---
number: 16188
title: psycopg2-binary docker build failure
type: issue
state: closed
author: sabinayakc
labels:
  - bug
assignees: []
created_at: 2025-10-08T14:10:37Z
updated_at: 2025-10-08T14:34:53Z
url: https://github.com/astral-sh/uv/issues/16188
synced_at: 2026-01-10T01:26:04Z
---

# psycopg2-binary docker build failure

---

_Issue opened by @sabinayakc on 2025-10-08 14:10_

### Summary

Hello,

Seeing below issue with Docker build. I only use the psycopg2-binary. 
```
 × Failed to build `psycopg2-binary==2.9.10`
5.924   ├─▶ The build backend returned an error
5.924   ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit
5.924       status: 1)
5.924 
5.924       [stdout]
5.924       running egg_info
5.924       writing psycopg2_binary.egg-info/PKG-INFO
5.924       writing dependency_links to psycopg2_binary.egg-info/dependency_links.txt
5.924       writing top-level names to psycopg2_binary.egg-info/top_level.txt
5.924 
5.924       [stderr]
5.924       /root/.cache/uv/builds-v0/.tmpsI18OY/lib/python3.14/site-packages/setuptools/dist.py:759:
5.924       SetuptoolsDeprecationWarning: License classifiers are deprecated.
5.924       !!
5.924 
5.924       
5.924       ********************************************************************************
5.924               Please consider removing the following classifiers in favor of a
5.924       SPDX license expression:
5.924 
5.924               License :: OSI Approved :: GNU Library or Lesser General Public
5.924       License (LGPL)
5.924 
5.924               See
5.924       https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license
5.924       for details.
5.924       
5.924       ********************************************************************************
5.924 
5.924       !!
5.924         self._finalize_license_expression()
5.924 
5.924       Error: pg_config executable not found.
5.924 
5.924       pg_config is required to build psycopg2 from source.  Please add the
5.924       directory
5.924       containing pg_config to the $PATH or specify the full executable path
5.924       with the
5.924       option:
5.924 
5.924           python setup.py build_ext --pg-config /path/to/pg_config build ...
5.924 
5.924       or with the pg_config option in 'setup.cfg'.
5.924 
5.924       If you prefer to avoid building psycopg2 from source, please install
5.924       the PyPI
5.924       'psycopg2-binary' package instead.
5.924 
5.924       For further information please check the 'doc/src/install.rst' file
5.924       (also at
5.924       <https://www.psycopg.org/docs/install.html>).
5.924 
5.924 
5.924       hint: This usually indicates a problem with the package or the build
5.924       environment.
5.924   help: `psycopg2-binary` (v2.9.10) was included because `goodspeedusa`
5.924         (v0.1.0) depends on `psycopg2-binary`
```

My docker file (pulling latest)
```
FROM ghcr.io/astral-sh/uv:bookworm-slim AS base

FROM base AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
WORKDIR /app
COPY uv.lock pyproject.toml /app/
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-install-project --no-dev
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-dev

FROM base
COPY --from=builder /app /app
ENV PATH="/app/.venv/bin:$PATH"
WORKDIR /app
EXPOSE 8000
CMD ["uv", "run", "fastapi", "run", "app/main.py", "--host", "0.0.0.0", "--port", "8000"]
```

Workaround. Downgraded to 0.8-bookworm-slim

```
FROM ghcr.io/astral-sh/uv:0.8-bookworm-slim AS base

FROM base AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
WORKDIR /app
COPY uv.lock pyproject.toml /app/
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-install-project --no-dev
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-dev

FROM base
COPY --from=builder /app /app
ENV PATH="/app/.venv/bin:$PATH"
WORKDIR /app
EXPOSE 8000
CMD ["uv", "run", "fastapi", "run", "app/main.py", "--host", "0.0.0.0", "--port", "8000"]
```



### Platform

WSL Ubuntu + Gitlab Ubuntu X86 (24)

### Version

latest (0.9)

### Python version

3.14

---

_Label `bug` added by @sabinayakc on 2025-10-08 14:10_

---

_Comment by @charliermarsh on 2025-10-08 14:12_

This is probably because the latest uv (v0.9) supports Python 3.14, and you're using it by default, and `psycopg2-binary` has no wheels for that Python version. I suggest using `.python-version` file to pin to Python 3.13 if your project isn't ready to upgrade.

---

_Comment by @sabinayakc on 2025-10-08 14:33_

Ahh, make sense. Thank you. I'll close this one since its issue with upstream. 

---

_Closed by @sabinayakc on 2025-10-08 14:34_

---
