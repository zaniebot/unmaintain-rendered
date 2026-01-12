```yaml
number: 13987
title: "[Dockerfile Mac M4 Pro] exec /app/.venv/bin/gunicorn: exec format error"
type: issue
state: closed
author: jakubbober
labels:
  - question
assignees: []
created_at: 2025-06-12T11:58:54Z
updated_at: 2025-07-18T23:45:19Z
url: https://github.com/astral-sh/uv/issues/13987
synced_at: 2026-01-12T16:01:41Z
```

# [Dockerfile Mac M4 Pro] exec /app/.venv/bin/gunicorn: exec format error

---

_@jakubbober_

### Summary

I wanted to start using `uv` more like a pro. Before, I was simply using it exactly like `pip` by just running `uv pip` instead of `pip` for faster installs. Then, I had a Dockerfile that looked as follows:

```docker
# Use an official Python runtime as a parent image
FROM python:3.12-slim

WORKDIR /app

# Install uv
RUN pip install --no-cache-dir uv

COPY requirements.txt /app

RUN uv pip install --system --no-cache -r requirements.txt

COPY . /app

EXPOSE 8000

# Run app when the container launches
CMD ["gunicorn", "-b", "0.0.0.0:8000", "-k", "uvicorn.workers.UvicornWorker", "--workers", "4", "--threads", "8", "--timeout", "900", "endpoint:app"]
```


To move away from `requirements.txt` and start using `uv.lock`, I switched my Dockerfile to the following:
```docker
# Use an official Python runtime as a parent image
FROM python:3.12-slim

WORKDIR /app

# Install uv
RUN pip install --no-cache-dir uv

COPY pyproject.toml /app
COPY uv.lock /app

RUN uv sync --locked

COPY . /app

EXPOSE 8000

ENV PATH="/app/.venv/bin:$PATH"

# Run app when the container launches
CMD ["gunicorn", "-b", "0.0.0.0:8000", "-k", "uvicorn.workers.UvicornWorker", "--workers", "4", "--threads", "8", "--timeout", "900", "endpoint:app"]
```

However, I'm then ending up with the following error after `docker-compose up`:
```
api-1  | exec /app/.venv/bin/gunicorn: exec format error
api-1 exited with code 255
```

I also tried some more complicated Dockerfile setups like [this](https://github.com/astral-sh/uv-docker-example) or [this](https://depot.dev/docs/container-builds/how-to-guides/optimal-dockerfiles/python-uv-dockerfile), but I was always ending up with the same error.

My hypothesis is that it's the fault of my Mac M4 Pro, but I'm not sure what to do about it. Any help would be appreciated!

### Platform

Darwin 24.5.0 arm64 (Mac M4 Pro)

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

Python 3.12.11

---

_Label `bug` added by @jakubbober on 2025-06-12 11:58_

---

_Comment by @zanieb on 2025-06-12 13:10_

Is your `.venv` directory being mounted? That'll break things since you're mounting a macOS environment into a Linux container.

See https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container and https://github.com/astral-sh/uv-docker-example/blob/6473a50c02f9c36dda8c29cf3e0b6fd84839d353/compose.yml#L18-L21

---

_Label `bug` removed by @zanieb on 2025-06-12 13:10_

---

_Label `question` added by @zanieb on 2025-06-12 13:10_

---

_Closed by @charliermarsh on 2025-07-16 13:46_

---

_Comment by @jakubbober on 2025-07-18 23:45_

Yep, thank was the issue indeed! Solved it by adding another `.venv` volume to my docker compose, so my `docker-compose.yml` ended up looking like this:

```yml
services:
  chat:
    build: .
    env_file:
      - .env
    ports:
      - '8000:8000'
    volumes:
      - .:/app
      - /app/.venv
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

Before my compose file was missing the `- /app/.venv` line, now it's mounting the entire app separately from the environment which works.
Thanks for the help and sorry for the late reply!

---
