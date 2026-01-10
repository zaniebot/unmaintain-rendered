```yaml
number: 3576
title: "Revert \"Use manylinux: auto for aarch64 builds (#3444)\""
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/fix-ssl-macos
created_at: 2024-05-14T13:39:17Z
updated_at: 2024-05-14T14:04:54Z
url: https://github.com/astral-sh/uv/pull/3576
synced_at: 2026-01-10T14:37:54Z
```

# Revert "Use manylinux: auto for aarch64 builds (#3444)"

---

_Pull request opened by @zanieb on 2024-05-14 13:39_

This reverts #3444 which appears to be the cause of SSL failures when building aarch64 Linux images on macOS hosts (see https://github.com/astral-sh/uv/issues/3570)

Closes https://github.com/astral-sh/uv/issues/3570

---

_Merged by @zanieb on 2024-05-14 13:51_

---

_Closed by @zanieb on 2024-05-14 13:51_

---

_Branch deleted on 2024-05-14 13:51_

---

_Comment by @zanieb on 2024-05-14 14:04_

I tested this by downloading the aarch64 wheel from this workflow and the the last release workflow.

Succeeds:
```
ARG PYTHON_VERSION=3.11
FROM python:${PYTHON_VERSION}-slim as base

RUN apt-get update
RUN apt-get install -y curl build-essential cmake

ADD ./uv-0.1.43-py3-none-manylinux_2_28_aarch64.whl ./uv-0.1.43-py3-none-manylinux_2_28_aarch64.whl
RUN pip install ./uv-0.1.43-py3-none-manylinux_2_28_aarch64.whl

ADD ./scripts/requirements/compiled/black.txt /requirements.txt

ENV RUST_LOG=trace
RUN uv pip install -r /requirements.txt --system -v
```


Fails:

```
ARG PYTHON_VERSION=3.11
FROM python:${PYTHON_VERSION}-slim as base

RUN apt-get update
RUN apt-get install -y curl build-essential cmake

ADD ./uv-0.1.43-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl ./uv-0.1.43-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
RUN pip install ./uv-0.1.43-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl

ADD ./scripts/requirements/compiled/black.txt /requirements.txt

ENV RUST_LOG=trace
RUN uv pip install -r /requirements.txt --system -v
```

---
