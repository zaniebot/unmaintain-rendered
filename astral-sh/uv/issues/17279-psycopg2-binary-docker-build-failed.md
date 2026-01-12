```yaml
number: 17279
title: psycopg2-binary docker build failed
type: issue
state: closed
author: blossomsg
labels:
  - bug
assignees: []
created_at: 2025-12-31T12:54:11Z
updated_at: 2026-01-01T07:06:03Z
url: https://github.com/astral-sh/uv/issues/17279
synced_at: 2026-01-12T16:02:48Z
```

# psycopg2-binary docker build failed

---

_@blossomsg_

### Summary

Hello,
I am facing an issue while building psycopg2 for docker

I referred the old ticket and compared all the versions software versions - https://github.com/astral-sh/uv/issues/16188
no luck, let me know if i have missed out any thing. 
Looking for suggestions/fix.

command
```
docker-compose up --build / docker-compose up
```

error log
```
Using CPython 3.12.12 interpreter at: /usr/local/bin/python3
Creating virtual environment at: .venv
Resolved 6 packages in 8ms
Building psycopg2==2.9.11
× Failed to build `psycopg2==2.9.11`
├─▶ The build backend returned an error
╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit
status: 1)

[stdout]
running egg_info
writing psycopg2.egg-info/PKG-INFO
writing dependency_links to psycopg2.egg-info/dependency_links.txt
writing top-level names to psycopg2.egg-info/top_level.txt

[stderr]
/root/.cache/uv/builds-v0/.tmpBw7IGr/lib/python3.12/site-packages/setuptools/dist.py:759:
SetuptoolsDeprecationWarning: License classifiers are deprecated.
!!


********************************************************************************
Please consider removing the following classifiers in favor of a
SPDX license expression:

License :: OSI Approved :: GNU Library or Lesser General Public
License (LGPL)

See
https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license
for details.

********************************************************************************

!!
self._finalize_license_expression()

Error: pg_config executable not found.

pg_config is required to build psycopg2 from source.  Please add the
directory
containing pg_config to the $PATH or specify the full executable path
with the
option:

python setup.py build_ext --pg-config /path/to/pg_config build ...

or with the pg_config option in 'setup.cfg'.

If you prefer to avoid building psycopg2 from source, please install
the PyPI
'psycopg2-binary' package instead.

For further information please check the 'doc/src/install.rst' file
(also at
<https://www.psycopg.org/docs/install.html>).


hint: This usually indicates a problem with the package or the build
environment.
help: `psycopg2` (v2.9.11) was included because `web-apps` (v0.1.0) depends
on `psycopg2` 
```


**Dockerfile**
```
# syntax=docker/dockerfile:1

# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Dockerfile reference guide at
# https://docs.docker.com/go/dockerfile-reference/

# Want to help us make this template better? Share your feedback here: https://forms.gle/ybq9Krt8jtBL3iCk7

FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Setup a non-root user
RUN groupadd --system --gid 999 nonroot \
    && useradd --system --gid 999 --uid 999 --create-home nonroot

WORKDIR /app

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr to avoid situations where
# the application crashes without emitting any logs due to buffering.
ENV PYTHONUNBUFFERED=1

# Enable bytecode compilation
ENV UV_LINK_MODE=copy

# Ensure installed tools can be executed out of the box
ENV UV_TOOL_BIN_DIR=/usr/local/bin

# Download dependencies as a separate step to take advantage of Docker's caching.
# Leverage a cache mount to /root/.cache/pip to speed up subsequent builds.
# Leverage a bind mount to Install the project's dependencies using the lockfile and settings
RUN --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-install-project

# Then, add the rest of the project source code and install it
# Installing separately from its dependencies allows optimal layer caching
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked

# Place executables in the environment at the front of the path
ENV PATH="/app/.venv/bin:$PATH"

# Use the non-root user to run our application
USER nonroot

# Expose the port that the application listens on.
EXPOSE 8000

# Run the application.
CMD ["uv", "run", "python", "manage.py", "runserver", "0.0.0.0:8000"]

```

**compose.yaml**
```
services:
  web:
    build: .
    command: uv run python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - 8000:8000
    depends_on:
      - db
  db:
    image: postgres:17
```

Thank You.

### Platform

Windows 11 Pro x64

### Version

uv 0.8.2 (21fadbcc1 2025-07-22)

### Python version

Python 3.12.0

---

_Label `bug` added by @blossomsg on 2025-12-31 12:54_

---

_Comment by @samypr100 on 2025-12-31 19:05_

Hello, psycopg2 does not seem provide wheels for platforms besides windows. See https://pypi.org/project/psycopg2/2.9.11/#files

Given this, uv will attempt to build from source like you're seeing. Since the build requirements for psycopg2 are not satisfied in the `-slim` variant you're using, it will result in build failures. I would suggest you add psycopg2-binary instead which does have wheels for linux, see https://pypi.org/project/psycopg2-binary/2.9.11/#files

Via `pysocpg2-binary`

```console
> docker run --rm -it ghcr.io/astral-sh/uv:0.8.2-python3.12-bookworm-slim bash -c 'pushd /root; uv init; uv add psycopg2==2.9.11'
Initialized project `root`
Using CPython 3.12.11 interpreter at: /usr/local/bin/python3.12
Creating virtual environment at: .venv
Resolved 2 packages in 265ms
Prepared 1 package in 353ms
Installed 1 package in 2ms
 + psycopg2-binary==2.9.11
```

Via `pysocpg2` (should work if you remove the `-slim`)

```console
> docker run --rm -it ghcr.io/astral-sh/uv:0.8.2-python3.12-bookworm-slim bash -c 'pushd /root; uv init; uv add psycopg2==2.9.11'
Initialized project `root`
Using CPython 3.12.11 interpreter at: /usr/local/bin/python3.12
Creating virtual environment at: .venv
Resolved 2 packages in 180ms
  × Failed to build `psycopg2==2.9.11`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running egg_info
      writing psycopg2.egg-info/PKG-INFO
      writing dependency_links to psycopg2.egg-info/dependency_links.txt
      writing top-level names to psycopg2.egg-info/top_level.txt

      [stderr]
      /root/.cache/uv/builds-v0/.tmp9bdSFt/lib/python3.12/site-packages/setuptools/dist.py:759:
      SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: GNU Library or Lesser General Public License (LGPL)

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()

      Error: pg_config executable not found.

      pg_config is required to build psycopg2 from source.  Please add the directory
      containing pg_config to the $PATH or specify the full executable path with the
      option:

          python setup.py build_ext --pg-config /path/to/pg_config build ...

      or with the pg_config option in 'setup.cfg'.

      If you prefer to avoid building psycopg2 from source, please install the PyPI
      'psycopg2-binary' package instead.

      For further information please check the 'doc/src/install.rst' file (also at
      <https://www.psycopg.org/docs/install.html>).


      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
        locking and syncing.
```

Notice in the build failure it says

> If you prefer to avoid building psycopg2 from source, please install the PyPI 'psycopg2-binary' package instead.

---

_Comment by @blossomsg on 2026-01-01 07:05_

Hey @samypr100 ,
Thanks that actually worked.
Thanks for the links and explanation.
Closing the ticket.

---

_Closed by @blossomsg on 2026-01-01 07:06_

---
