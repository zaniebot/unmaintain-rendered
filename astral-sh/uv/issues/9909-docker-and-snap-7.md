```yaml
number: 9909
title: Docker and snap-7
type: issue
state: open
author: MMartin09
labels:
  - needs-mre
assignees: []
created_at: 2024-12-15T12:19:09Z
updated_at: 2025-01-13T19:03:12Z
url: https://github.com/astral-sh/uv/issues/9909
synced_at: 2026-01-12T16:00:02Z
```

# Docker and snap-7

---

_@MMartin09_

Hi, I am trying to Dockerize my project. However, I am always receiving an error from the Python snap7 library. 

I am using this Docker file 
```
FROM python:3.12.7-slim-bullseye

ENV PYTHONUNBUFFERED=1

WORKDIR /app/

RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    libc6-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install uv
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#installing-uv
COPY --from=ghcr.io/astral-sh/uv:0.5.9 /uv /uvx /bin/

# Place executables in the environment at the front of the path
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment
ENV PATH="/app/.venv/bin:$PATH"

# Compile bytecode
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#compiling-bytecode
ENV UV_COMPILE_BYTECODE=1

# uv Cache
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#caching
ENV UV_LINK_MODE=copy

# Install dependencies
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project

ENV PYTHONPATH=/app

COPY ./pyproject.toml ./uv.lock /app/

COPY ./app /app/app

# Sync the project
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync

CMD ["fastapi", "run", "--workers", "2", "app/main.py", "--port", "8000", "--host", "0.0.0.0"]
```

And these are my dependencies:
```
requires-python = ">=3.12"
dependencies = [
    "pydantic>=2.9.2",
    "pydantic-settings>=2.6.1",
    "sqlmodel>=0.0.22",
    "fastapi[standard]>=0.115.5",
    "redis>=5.2.0",
    "psycopg[binary]>=3.2.3",
    "httpx>=0.27.2",
    "python-snap7>=2.0.2",
]
```

Executing the Docker I am receiving the error:
```
2024-12-15 13:14:43 This probably means you are installing python-snap7 from source. When no binary wheel is found for you architecture, pip
2024-12-15 13:14:43 install falls back on a source install. For this to work, you need to manually install the snap7 library, which
2024-12-15 13:14:43 python-snap7 uses under the hood.
2024-12-15 13:14:43 
2024-12-15 13:14:43 The shortest path to success is to try to get a binary wheel working. Probably you are running on an unsupported
2024-12-15 13:14:43 platform or python version. You are running:
2024-12-15 13:14:43 
2024-12-15 13:14:43 machine: x86_64
2024-12-15 13:14:43 system: Linux
2024-12-15 13:14:43 python version: 3.12.7
2024-12-15 13:14:43 
2024-12-15 13:15:09 Exception ignored in: <function Client.__del__ at 0x7fd21840bce0>
2024-12-15 13:15:09 Traceback (most recent call last):
2024-12-15 13:15:09   File "/app/.venv/lib/python3.12/site-packages/snap7/client.py", line 74, in __del__
2024-12-15 13:15:09     self.destroy()
2024-12-15 13:15:09   File "/app/.venv/lib/python3.12/site-packages/snap7/client.py", line 93, in destroy
2024-12-15 13:15:09     if self._lib and self._s7_client is not None:
2024-12-15 13:15:09        ^^^^^^^^^
2024-12-15 13:15:09 AttributeError: 'Logo' object has no attribute '_lib'
```

However, if I install the packages without `uv` everything works fine.

---

_Comment by @samypr100 on 2024-12-15 19:49_

I didn't encounter issues when trying with a smaller example with the dependencies you provided. I also tried a similar one with `python:3.12.7-slim-bullseye`. Could you provide a reproducible `pyproject.toml` and `uv.lock` for your example?

```shell
docker run \
--platform linux/x86_64 \
-it \
--rm \
ghcr.io/astral-sh/uv:bookworm /bin/bash -c \
"uv venv; uv pip install pydantic>=2.9.2 pydantic-settings>=2.6.1 sqlmodel>=0.0.22 fastapi[standard]>=0.115.5 redis>=5.2.0 psycopg[binary]>=3.2.3 httpx>=0.27.2 python-snap7>=2.0.2"
```

---

_Label `needs-mre` added by @samypr100 on 2024-12-15 19:49_

---

_Comment by @MMartin09 on 2024-12-21 16:20_

@samypr100 I have tested it multiple times and it always has the same problems. 
Here are the files: https://gist.github.com/MMartin09/04f2b4901f1522885f7c01709f934ff6

---

_Comment by @samypr100 on 2024-12-21 20:56_

@MMartin09 

I copied both your files and ran `uv sync` without issues. If the issue is only present when running your app, I would unfortunately still not have a way to reproduce it without an app.py MRE.

```
$ uv sync
Using CPython 3.12.8
Creating virtual environment at: .venv
Resolved 53 packages in 9ms
Prepared 47 packages in 1.23s
Installed 50 packages in 36ms
 + annotated-types==0.7.0
 + anyio==4.6.2.post1
 + certifi==2024.8.30
 + cfgv==3.4.0
 + click==8.1.7
 + distlib==0.3.9
 + dnspython==2.7.0
 + email-validator==2.2.0
 + fastapi==0.115.5
 + fastapi-cli==0.0.5
 + filelock==3.16.1
 + greenlet==3.1.1
 + h11==0.14.0
 + httpcore==1.0.7
 + httptools==0.6.4
 + httpx==0.27.2
 + identify==2.6.2
 + idna==3.10
 + jinja2==3.1.4
 + markdown-it-py==3.0.0
 + markupsafe==3.0.2
 + mdurl==0.1.2
 + nodeenv==1.9.1
 + platformdirs==4.3.6
 + pre-commit==4.0.1
 + psycopg==3.2.3
 + psycopg-binary==3.2.3
 + pydantic==2.9.2
 + pydantic-core==2.23.4
 + pydantic-settings==2.6.1
 + pygments==2.18.0
 + python-dotenv==1.0.1
 + python-multipart==0.0.17
 + python-snap7==2.0.2
 + pyyaml==6.0.2
 + redis==5.2.0
 + rich==13.9.4
 + ruff==0.7.4
 + shellingham==1.5.4
 + sniffio==1.3.1
 + sqlalchemy==2.0.36
 + sqlmodel==0.0.22
 + starlette==0.41.2
 + typer==0.13.0
 + typing-extensions==4.12.2
 + uvicorn==0.32.0
 + uvloop==0.21.0
 + virtualenv==20.27.1
 + watchfiles==0.24.0
 + websockets==14.1
```

---

_Comment by @MMartin09 on 2025-01-13 19:03_

Yes, building is no problem. 

Only when I run the code. 
For example: 
```python 
import snap7


def main() -> None:
    plc = snap7.logo.Logo()
    plc.connect('192.168.0.241', 0x3000, 0x2000)
    
    plc.write('V1064.0', False)

    print('DO1: ' + str(plc.read('V1064.0')))
    print('DO2: ' + str(plc.read('V1064.1')))
    print('DO3: ' + str(plc.read('V1064.2')))


if __name__ == '__main__':
    main()

```

---
