---
number: 16113
title: Pre-installing heavy dependencies in separate Dockerfile layer
type: issue
state: closed
author: evakkuri
labels:
  - question
assignees: []
created_at: 2025-10-03T09:33:10Z
updated_at: 2025-12-03T03:58:06Z
url: https://github.com/astral-sh/uv/issues/16113
synced_at: 2026-01-10T01:26:03Z
---

# Pre-installing heavy dependencies in separate Dockerfile layer

---

_Issue opened by @evakkuri on 2025-10-03 09:33_

### Question

Hi, and thanks for the awesome system that UV is proving to be!

**Context:** 

We are building a machine learning project using Anyscale, Ray, and PyTorch. For running our model training in Anyscale, we use their GPU-enabled base image, as of writing [anyscale/ray:2.49.1-slim-py311-cu128](https://hub.docker.com/layers/anyscale/ray/2.49.1-slim-py311-cu128/images/sha256-76614d7753af897b13a859133dd019cd747174292a12399a2f2dd53af1479181). We then add our project files in a few stages according to [UV instructions on caching](https://docs.astral.sh/uv/guides/integration/docker/#caching). I have attached our Dockerfile below.

**Issue:**

The resulting image is very big, over 7GB in size, most likely due to CUDA dependencies. This makes creating and pushing the image very slow, the process can take over 10min on a very high-end MacBook Pro. As we install all of our external dependencies in one go through various pyproject.toml files, this means that any time we make any change in those files or our lockfile updates, we'll need to go through the full 10min process again, even if the change did not touch CUDA deps.

**Question:**

Is it possible with UV to separate our heavy dependencies to a separate layer? So that we might install e.g. PyTorch, Ray and the resulting CUDA deps in one layer and then that would be cached. We have tried some approaches, but no success yet.

Any ideas greatly appreciated!

Our Dockerfile is as follows:

```
# syntax=docker/dockerfile:1
# check=skip=InvalidDefaultArgInFrom

# NOTE: The above warning is due to the use of ARG in the FROM instruction.
# We want the build to fail if the URI does not make sense, so ignoring the warning is fine.

ARG ANYSCALE_BASE_IMAGE_URI

FROM ${ANYSCALE_BASE_IMAGE_URI} AS base

WORKDIR /home/ray/default

# Skip dev dependencies in UV by default
ENV UV_NO_DEV=1

# Use uv virtual environment instead of anaconda
ENV VIRTUAL_ENV=/home/ray/default/.venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
ENV PYTHONPATH=/home/ray/default

# Configure UV caching for Docker builds
ENV UV_LINK_MODE=copy
ENV UV_CACHE_DIR=/home/ray/.cache/uv
ENV UV_PYTHON_CACHE_DIR=/home/ray/.cache/uv/python

RUN rm -rf /home/ray/.bashrc

FROM base AS heavy-deps

# Install same versions of heavy deps as in model training deps
# [TODO] Fetch versions dynamically from pyproject.toml
RUN uv venv && \
    uv pip install \
    torch==2.8.0 \
    lightning==2.5.3 \
    ray[data,train,tune,default]==2.49.1

### Production dependencies
FROM heavy-deps AS prod-deps

# Base image uses these as default user, group, and starting directory
ARG OWNER=ray:users

# Copy all pyprojects first for installing dependencies. This is required for the --locked
# flag to work correctly. Only specific packages are installed in the end.
# NOTE: If you add more packages, remember to add them here!
COPY --chown=${OWNER} uv.lock pyproject.toml ./
COPY --chown=${OWNER} packages/chronos_client/pyproject.toml packages/chronos_client/pyproject.toml
COPY --chown=${OWNER} packages/conf/pyproject.toml packages/conf/pyproject.toml
COPY --chown=${OWNER} packages/input_data/pyproject.toml packages/input_data/pyproject.toml
COPY --chown=${OWNER} packages/model_training/pyproject.toml packages/model_training/pyproject.toml
COPY --chown=${OWNER} packages/preprocessing/pyproject.toml packages/preprocessing/pyproject.toml
COPY --chown=${OWNER} packages/postprocessing/pyproject.toml packages/postprocessing/pyproject.toml

# Install production dependencies
RUN --mount=type=cache,target=/home/ray/.cache/uv,uid=1000,gid=100 \
    uv sync \
    --locked \
    --no-install-workspace \
    --package model_training

# Run tests for packages required by Anyscale
RUN echo "Testing required packages..." && \
    python -c "import ray" && \
    ray --version && \
    jupyter --version && \
    anyscale --version

### Job environment
FROM prod-deps AS job

# Add local code
COPY --chown=${OWNER} . ./

# Install with also local deps
RUN --mount=type=cache,target=/home/ray/.cache/uv,uid=1000,gid=100 \
    uv sync --locked --package model_training
```

### Platform

Darwin 25.0.0 arm64 for local envs, some Ubuntu x86 for CICD

### Version

uv 0.8.22 (Homebrew 2025-09-23)

---

_Label `question` added by @evakkuri on 2025-10-03 09:33_

---

_Comment by @zanieb on 2025-10-21 18:19_

I think you can workaround this by adding your dependencies to a `heavy` dependency group then exporting them in a different layer?

e.g., in a trivialized example

```Dockerfile
FROM astral/uv:bookworm AS export
RUN uv python install 3.14
ADD pyproject.toml uv.lock /
RUN uv export --only-group heavy --output-file pylock.heavy.toml

FROM astral/uv:bookworm
WORKDIR /app
RUN uv venv -p 3.14
COPY --from=export /pylock.heavy.toml /app/pylock.heavy.toml
RUN uv pip install -r pylock.heavy.toml
ADD . /app
RUN uv sync
```

(You might need to normalize the timestamps)

Does that make sense?

---

_Closed by @charliermarsh on 2025-12-03 03:58_

---
