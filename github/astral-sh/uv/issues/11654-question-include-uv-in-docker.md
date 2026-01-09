---
number: 11654
title: "[Question] include uv in docker"
type: issue
state: closed
author: mru4913
labels:
  - question
assignees: []
created_at: 2025-02-20T06:13:08Z
updated_at: 2025-02-20T09:10:55Z
url: https://github.com/astral-sh/uv/issues/11654
synced_at: 2026-01-07T13:12:18-06:00
---

# [Question] include uv in docker

---

_Issue opened by @mru4913 on 2025-02-20 06:13_

### Question


I attempted to build a Docker image for my project, which is managed by `uv`. When I attempt to launch a new container by mounting my local project, it consistently deletes my local `.env` file and reinstalls dependencies in a new `.env` file. Does anyone have any idea what might be going wrong here?

I also attempted the method detailed in the [documentation](https://docs.astral.sh/uv/guides/integration/docker/#installing-a-package), yet without success. 

```
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3 -> python
Using CPython 3.11.11 interpreter at: /usr/bin/python3.11
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```

A simplified docker file below.
```
FROM nvidia/cuda:12.4.1-cudnn-runtime-ubuntu20.04  

ENV DEBIAN_FRONTEND=noninteractive \
    PATH="/root/.local/bin/:$PATH"

RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common curl && \
    add-apt-repository -y ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    python3.11 \
    python3.11-venv \
    python3.11-dev \
    libgl1-mesa-glx && \
    curl -sS https://bootstrap.pypa.io/get-pip.py | python3.11 && \
    apt-get autoremove -y && \
    ln -fs /usr/bin/python3.11 /usr/bin/python && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /app
COPY pyproject.toml uv.lock /app/

RUN pip install uv
RUN uv pip install --system --no-cache-dir . \
    --index-url https://pypi.org/simple/ \
```

cc @zanieb @charliermarsh 

### Platform

linux

### Version

0.5.22

---

_Label `question` added by @mru4913 on 2025-02-20 06:13_

---

_Closed by @mru4913 on 2025-02-20 09:10_

---
