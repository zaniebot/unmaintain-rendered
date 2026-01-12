```yaml
number: 16289
title: Ability to cache Python downloads in Docker build
type: issue
state: open
author: merlinz01
labels:
  - question
assignees: []
created_at: 2025-10-13T23:07:27Z
updated_at: 2025-10-14T17:34:44Z
url: https://github.com/astral-sh/uv/issues/16289
synced_at: 2026-01-12T16:02:28Z
```

# Ability to cache Python downloads in Docker build

---

_@merlinz01_

### Question

Is there a way to cache what is downloaded for managed Python installations? As far as I can tell, uv either downloads and extracts in-memory to `UV_PYTHON_INSTALL_DIR`, or downloads to a temp file, extracts, and then deletes it. Neither would make it possible to cache the Python download in Docker with `RUN --mount=type=cache`. So with `UV_CACHE_DIR` cached, even though the package installation is very fast, it has to download Python every time the packages are installed, slowing down the build.

It doesn't work to cache-mount `UV_PYTHON_INSTALL_DIR` because then the Python binary isn't included in the final image.

This is the relevant part of the Dockerfile I'm using:

```dockerfile
# syntax=docker/dockerfile:1-labs
FROM debian:trixie-slim
RUN apt-get update && \
    apt-get install -y ca-certificates git && \
    update-ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
COPY --from=ghcr.io/astral-sh/uv:0.7 /uv /usr/bin/uv
RUN useradd myuser -d /app -M -U -u 1000
USER myuser
WORKDIR /app
COPY pyproject.toml uv.lock ./
RUN mkdir -p .cache/uv
RUN --mount=type=cache,uid=1000,gid=1000,target=/app/.cache/uv \
    uv sync --compile-bytecode --frozen --no-dev --link-mode=copy
```

As you can see it takes 2.8 seconds downloading Python:

```
 > [stage-0  9/12] RUN --mount=type=cache,uid=1000,gid=1000,target=/app/.cache/uv     uv sync --compile-bytecode --frozen --no-dev --link-mode=copy:
0.390 Downloading cpython-3.13.5-linux-x86_64-gnu (download) (33.8MiB)
3.173  Downloading cpython-3.13.5-linux-x86_64-gnu (download)
3.305 Using CPython 3.13.5
3.305 Creating virtual environment at: .venv
3.529 Installed 102 packages in 208ms
4.443 Bytecode compiled 3072 files in 912ms
4.443  + aerich==0.9.2
4.443  + aiofiles==24.1.0
4.443  + aiohappyeyeballs==2.6.1
4.443  + aiohttp==3.12.3
4.443  + aiosignal==1.3.2
...
```

What I would like is if there was something like `$UV_PYTHON_DOWNLOAD_CACHE_DIR` (default to `$UV_CACHE_DIR/python-download-archives`) that is where all the Python distributions (`cpython-3.13.5-linux-x86_64-gnu.tar.gz` etc) are downloaded to and left intact, then used for subsequent Python installs.

Possibly related:
#13304
#7865
#8025
#14265
#6212
https://github.com/astral-sh/setup-uv/pull/621

### Platform

Linux 6.1.0-39-amd64 x86_64 GNU/Linux

### Version

uv 0.7.22

---

_Label `question` added by @merlinz01 on 2025-10-13 23:07_

---

_Comment by @konstin on 2025-10-14 10:27_

There's `UV_PYTHON_CACHE_DIR`, but for docker builds, you can run `uv python install 3.13` in a separate step before `COPY pyproject.toml uv.lock ./` (potentially after copying the `.python_version` file), so the step gets cached in docker.

---

_Comment by @merlinz01 on 2025-10-14 12:32_

> There's `UV_PYTHON_CACHE_DIR`

Hey, that seems to do the trick. I only wish it were better documented, as it's only listed in the docs for environment variables. What is the default location for `UV_PYTHON_CACHE_DIR`?

---

_Comment by @zanieb on 2025-10-14 12:33_

There is no default location right now.

---

_Comment by @merlinz01 on 2025-10-14 12:34_

Does that mean that if `UV_PYTHON_CACHE_DIR` is unset, uv extracts on-the-fly to the target directory without saving the archive to a file?

---

_Comment by @merlinz01 on 2025-10-14 15:59_

I think that is the case, in this code:

https://github.com/astral-sh/uv/blob/8eada1685ccb4bcd63d78bcc808ab23a4585aa7f/crates/uv-python/src/downloads.rs#L1041-L1127

---

_Comment by @konstin on 2025-10-14 16:10_

While you can use `UV_PYTHON_CACHE_DIR`, I do recommend using `uv python install ...` in an early cached layer, it's much better suited to the task.

---

_Comment by @zanieb on 2025-10-14 17:17_

> Does that mean that if UV_PYTHON_CACHE_DIR is unset, uv extracts on-the-fly to the target directory without saving the archive to a file?

Yeah, that is the case.

I'll probably turn on the cache directory by default at some point? We just haven't yet. It's unclear if it's beneficial enough.

---

_Comment by @merlinz01 on 2025-10-14 17:34_

> While you can use `UV_PYTHON_CACHE_DIR`, I do recommend using `uv python install ...` in an early cached layer, it's much better suited to the task.

Good point, yet being able to cache Python downloads adds efficiency to the build for whenever a earlier yet layer is invalidated.

> I'll probably turn on the cache directory by default at some point? We just haven't yet. It's unclear if it's beneficial enough.

For this use case, I'm happy to set an environment variable since it's all specified in the Dockerfile and not something I have to type out often.

---
