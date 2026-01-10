```yaml
number: 3570
title: "`uv pip install` fails from inside docker."
type: issue
state: closed
author: ArshanKhanifar
labels: []
assignees: []
created_at: 2024-05-14T06:38:17Z
updated_at: 2024-05-14T16:53:32Z
url: https://github.com/astral-sh/uv/issues/3570
synced_at: 2026-01-10T05:31:37Z
```

# `uv pip install` fails from inside docker.

---

_Issue opened by @ArshanKhanifar on 2024-05-14 06:38_

On my mac the install works just fine. 

In docker though, I'm getting this error:
```
 => ERROR [builder  9/10] RUN --mount=type=ssh /root/.cargo/bin/uv pip install --system --no-cache -r requirements.txt   2.5s
------
 > [builder  9/10] RUN --mount=type=ssh /root/.cargo/bin/uv pip install --system --no-cache -r requirements.txt:
2.433 error: error sending request for url (https://northamerica-northeast2-python.pkg.dev/private-pypi-418615/ritual-pypi/simple/huggingface-hub/)
2.433   Caused by: client error (Connect)
2.433   Caused by: invalid peer certificate: BadSignature
```

This is my dockerfile:
```
# syntax=docker/dockerfile:1

# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Dockerfile reference guide at
# https://docs.docker.com/engine/reference/builder/

ARG PYTHON_VERSION=3.11
FROM python:${PYTHON_VERSION}-slim as base

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr to avoid situations where
# the application crashes without emitting any logs due to buffering.
ENV PYTHONUNBUFFERED=1
ENV RUNTIME docker

WORKDIR /app

# Create a non-privileged user that the frenrug will run under.
# See https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser

RUN apt-get update
RUN apt-get install -y curl

# Install UV
ADD --chmod=755 https://astral.sh/uv/install.sh /install.sh
RUN /install.sh && rm /install.sh

# Download dependencies as a separate step to take advantage of Docker's caching.
# Leverage a cache mount to /root/.cache/uv to speed up subsequent builds.
# Leverage a bind mount to requirements to avoid having to copy them into
# into this layer.
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=requirements.txt,target=requirements.txt \
    /root/.cargo/bin/uv pip install --system -r requirements.txt

# Install some executables
RUN apt-get update \
    && apt-get install -y curl procps sysstat ifstat \
    && rm -rf /var/lib/apt/lists/*

# Copy the source code into the container.
COPY src/ src/
COPY entrypoint.sh .

# Run the application.
ENTRYPOINT ["/app/entrypoint.sh"]
```

Since I'm installing `uv` every time, it must use the latest version of `uv`. 
It's using python's official docker, so I'm assuming the certificates are outdated whenever installing from a debian environment? 

---

_Comment by @smetanakarel on 2024-05-14 07:51_

Same issue here, latest MacOS 14.5. It worked yesterday. Problem is not related with used image (tried Debian, Rockylinux).

---

_Comment by @Max0u on 2024-05-14 09:03_

I have the same issue, small workaround is by using yesterday version 
```
RUN curl -LsSf https://astral.sh/uv/install.sh -o install.sh
RUN sed -i 's/0.1.43/0.1.42/g' install.sh
RUN sh install.sh
```
if anyone has a better way to set the version I would gladly take it 

---

_Comment by @yyovkov on 2024-05-14 10:34_

Following @Max0u experiment, can install older version directly:
```
pip install uv==0.1.42
````

---

_Comment by @jsch-adt on 2024-05-14 11:21_

This works if you're already using the curl -> sh install method and don't want to make any other changes:
`curl -LsSf https://github.com/astral-sh/uv/releases/download/0.1.42/uv-installer.sh | sh`

---

_Comment by @zanieb on 2024-05-14 12:19_

Looking into this...

---

_Comment by @zanieb on 2024-05-14 12:27_

I did not reproduce this with

```Dockerfile
ARG PYTHON_VERSION=3.11
FROM python:${PYTHON_VERSION}-slim as base

RUN apt-get update
RUN apt-get install -y curl

ADD --chmod=755 https://astral.sh/uv/install.sh /install.sh
RUN /install.sh && rm /install.sh

RUN /root/.cargo/bin/uv pip install anyio --system
```

---

_Comment by @jsch-adt on 2024-05-14 12:30_

In my case, it fails on a different library in my requirements.txt file every time I run install.

---

_Comment by @zanieb on 2024-05-14 12:34_

Okay I'll try more requirements, but you might just be seeing the first request to "complete" which can be non-deterministic since we run them concurrently.

Does the `--no-native-tls` or `--native-tls` flag have any effect?

---

_Comment by @zanieb on 2024-05-14 12:38_

Has anyone seen this behavior on a host machine other than macOS?

(Adding more requirements did not change behavior for me)

---

_Comment by @zanieb on 2024-05-14 12:42_

Okay I've confirmed

1. _Does not_ occur on Linux host machines
2. _Does_ reproduce on a macOS host machine

I'll look into the cause now.

---

_Comment by @rossmacarthur on 2024-05-14 13:00_

I'm using 

```
ADD --chmod=755 https://github.com/astral-sh/uv/releases/download/0.1.42/uv-installer.sh /uv-installer.sh
RUN /uv-installer.sh && rm /uv-installer.sh
```

For now

---

_Comment by @rossmacarthur on 2024-05-14 13:03_

@zanieb https://github.com/astral-sh/uv/pull/3444 looks like it could possibly affect SSL

---

_Closed by @zanieb on 2024-05-14 13:51_

---

_Comment by @zanieb on 2024-05-14 13:51_

I'll release a revert to that — it is indeed suspicious.

---

_Comment by @zanieb on 2024-05-14 14:20_

By downloading some intermediate artifacts I've confirmed reverting that resolves the issue. v0.1.44 is on its way out.

---

_Comment by @ArshanKhanifar on 2024-05-14 15:41_

It's fixed for me now. Re-running the same docker image today works.

---

_Comment by @cr3ative on 2024-05-14 16:49_

Thank you for this quick turnaround! I also pinned to previous to resolve, and was wondering why it wasn't affecting the Intel nodes. Makes sense!

---

_Comment by @zanieb on 2024-05-14 16:53_

Thanks for checking back in! Great to know it's resolved.

---
