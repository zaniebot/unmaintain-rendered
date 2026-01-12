```yaml
number: 15282
title: How to install local dependencies and intransitive local depenencies
type: issue
state: open
author: rafikg
labels:
  - question
assignees: []
created_at: 2025-08-14T14:49:01Z
updated_at: 2025-08-14T17:35:42Z
url: https://github.com/astral-sh/uv/issues/15282
synced_at: 2026-01-12T16:02:07Z
```

# How to install local dependencies and intransitive local depenencies

---

_@rafikg_

### Question

I am dealing with structure:
```
pyproject.toml
uv.lock
apps/
serviceA/serviceA
serviceB/serviceB
serviceC/serviceC
```

pyproject.toml:
```
[project]
name = "project"
version = "0.1.0"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["shared_dep1", "shared_dep2", "shared_dep3"] # shareable dependencies across all services.
[tool.uv.sources]
serviceA = { workspace = true }
serviceB= { workspace = true }
serviceC = { workspace = true }
[tool.uv.workspace]
members = [
   "apps/serviceA",
   "apps/serviceB",
   "apps/serviceC",
[dependency-groups]
dev = [
    "serviceA",
    "serviceB",
    "serviceC"]
```

**pyproject.toml serviceA**

```
[project]
name = "serviceA"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.10"
dependencies = ["colorama==0.4.6"]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

**pyproject.toml serviceB**

```
[project]
name = "serviceB"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.10"
dependencies = [serviceA, depb-1, dep-b2]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
[tool.uv.sources]
serviceA = { workspace=true }
```

**pyproject.toml serviceC**

```
[project]
name = "serviceC"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.10"
dependencies = [serviceB, depc-1, depc-2]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
[tool.uv.sources]
serviceB = { workspace=true }
```
Each service will be shipped in a docker image and deployed on a lambda function.

I implemented a solution that works but have one limitation:
1. I have to copy manually all the local dependency even the transitive ones. I think a solution for that is to add a script to copy all the dependency that are exist in requirements.txt and under ./apps/ but I am wondering if there is a smarter way!


**Here is my solution for serviceC:**
```
# modified from https://docs.astral.sh/uv/guides/integration/aws-lambda/#workspace-support
# uv binary
FROM ghcr.io/astral-sh/uv:0.8.4 AS uv

# First, bundle the dependencies into the task root.
FROM public.ecr.aws/lambda/python:3.12 AS builder
WORKDIR /build
# Enable bytecode compilation, to improve cold-start performance.
ENV UV_COMPILE_BYTECODE=1 
# Disable installer metadata, to create a deterministic layer.
ENV UV_NO_INSTALLER_METADATA=1
# Enable copy mode to support bind mount caching.
ENV UV_LINK_MODE=copy           

# PASS 1: only 3rd-party wheels (highly cacheable)
RUN --mount=from=uv,source=/uv,target=/usr/local/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv export --package service \
              --frozen --no-dev --no-editable --no-emit-workspace \
              -o req.txt && \
    uv pip install -r req.txt --target "${LAMBDA_TASK_ROOT}"

# PASS 2: Copy source files and install workspace packages
RUN --mount=from=uv,source=/uv,target=/usr/local/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=bind,source=apps/serviceA,target=apps/serviceA\
    --mount=type=bind,source=apps/serviceB,target=apps/serviceB\ 
    --mount=type=bind,source=apps/serviceC,target=apps/serviceC\
    uv export --package serviceC \
              --frozen --no-dev --no-editable \
              -o req.txt && \
    uv pip install -r req.txt --target "${LAMBDA_TASK_ROOT}"

###############################################################################
# runtime – minimal Lambda image
###############################################################################
FROM public.ecr.aws/lambda/python:3.12

# Bring in site-packages + any workspace wheels produced above
COPY --from=builder ${LAMBDA_TASK_ROOT} ${LAMBDA_TASK_ROOT}

# Copy *source* for both workspace packages to be absolutely sure
COPY apps/serviceA/serviceA${LAMBDA_TASK_ROOT}/serviceA
COPY apps/serviceA/serviceB${LAMBDA_TASK_ROOT}/serviceB
COPY apps/serviceA/serviceC${LAMBDA_TASK_ROOT}/serviceC
CMD ["serviceC.main.handler"]
```
How to deal with the limitations ?

### Platform

ubuntu

### Version

24.04.5

---

_Label `question` added by @rafikg on 2025-08-14 14:49_

---
