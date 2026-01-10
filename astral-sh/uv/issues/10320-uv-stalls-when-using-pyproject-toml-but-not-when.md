---
number: 10320
title: Uv stalls when using pyproject.toml, but not when using installing using cli
type: issue
state: open
author: vedantroy
labels:
  - question
assignees: []
created_at: 2025-01-06T06:21:52Z
updated_at: 2025-01-06T14:51:33Z
url: https://github.com/astral-sh/uv/issues/10320
synced_at: 2026-01-10T01:24:52Z
---

# Uv stalls when using pyproject.toml, but not when using installing using cli

---

_Issue opened by @vedantroy on 2025-01-06 06:21_

This works:
```
FROM nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends build-essential curl ca-certificates git imagemagick libgl1-mesa-glx

ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/root/.local/bin/:$PATH"
RUN uv python install 3.11

RUN uv venv /venv
ENV VIRTUAL_ENV=/venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# COPY pyproject.toml .
RUN uv pip install --verbose boto3==1.35.91 boto3-stubs[s3]==1.35.91 anthropic==0.42.0 tenacity==9.0.0 "redis[hiredis]>=5.2.1" "rollbar>=1.1.0" "python-dotenv>=1.0.0" "py3nvml>=0.2.7"
```

But, if I switch up the Dockerfile, I get stuck at the following:
```
 => [ 8/14] RUN uv pip install .[local] --verbose                                                                                                                                                                         4.6s
 => => # DEBUG Identified uncached distribution: setuptools==75.7.0                                                                                                                                                           
 => => # DEBUG Downloading and building requirement for build: setuptools==75.7.0                                                                                                                                             
 => => # DEBUG No cache entry for: https://files.pythonhosted.org/packages/4e/6e/abdfaaf5c294c553e7a81cf5d801fbb4f53f5c5b6646de651f92a2667547/setuptools-75.7.0-py3-none-any.whl                                              
 => => # DEBUG Installing build requirement: setuptools==75.7.0                                                                                                                                                               
 => => # DEBUG Creating PEP 517 build environment                                                                                                                                                                             
 => => # DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`  
```

Here's the new Dockerfile:
```
FROM nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends build-essential curl ca-certificates git imagemagick libgl1-mesa-glx

ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/root/.local/bin/:$PATH"
RUN uv python install 3.11

RUN uv venv /venv
ENV VIRTUAL_ENV=/venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

COPY pyproject.toml .
RUN uv pip install .[local] --verbose
# RUN uv pip install --verbose boto3==1.35.91 boto3-stubs[s3]==1.35.91 anthropic==0.42.0 tenacity==9.0.0 "redis[hiredis]>=5.2.1" "rollbar>=1.1.0" "python-dotenv>=1.0.0" "py3nvml>=0.2.7"
```
And here's the pyproject.toml:

```
[project]
name = "ml-stuff"
version = "0.1.0"
description = "Server"
requires-python = ">=3.9"
dependencies = [
    "boto3==1.35.91",
    "boto3-stubs[s3]==1.35.91",
    "anthropic==0.42.0",
    "tenacity==9.0.0",
    "redis[hiredis]>=5.2.1",
    "rollbar>=1.1.0",
]

# Install with uv pip install .[local]
[project.optional-dependencies]
local = [
    # "uvicorn==0.34.0",
    # "click==8.1.7",
    # "ngrok>=0.9.0",
    "python-dotenv>=1.0.0",
    "py3nvml>=0.2.7",
]
```


---

_Renamed from "Getting stuck when installing using pyproject.tomlT" to "Uv stalls when using pyproject.toml, but not when using installing using cli" by @vedantroy on 2025-01-06 06:22_

---

_Comment by @charliermarsh on 2025-01-06 14:51_

I think there's something strange going on in the image itself. If you run `uv pip install .[local] --verbose`, we have to build the project at `.` from source to install it. And there must be content at the repo root that `setuptools` is then trying to include.

Here are two alternatives that work.

First, if you just want the dependencies, and not the code in `ml-stuff` itself, change it to:

```dockerfile
RUN uv pip install "pyproject.toml[local]" --verbose
```

Second, you can copy the `pyproject.toml` into a dedicated directory, to avoid building whatever is in the image root:

```dockerfile
COPY pyproject.toml /app/pyproject.toml
RUN uv pip install /app[local] --verbose
```

---

_Label `question` added by @charliermarsh on 2025-01-06 14:51_

---

_Comment by @charliermarsh on 2025-01-06 14:51_

(Thanks for the clear reproduction.)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-06 14:51_

---
