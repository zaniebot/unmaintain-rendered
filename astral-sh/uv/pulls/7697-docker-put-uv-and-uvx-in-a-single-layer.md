```yaml
number: 7697
title: "Docker: put uv and uvx in a single layer"
type: pull_request
state: merged
author: akx
labels:
  - releases
assignees: []
merged: true
base: main
head: docker-uv-uvx-layers
created_at: 2024-09-26T07:16:44Z
updated_at: 2024-09-26T15:04:12Z
url: https://github.com/astral-sh/uv/pull/7697
synced_at: 2026-01-12T16:07:57Z
```

# Docker: put uv and uvx in a single layer

---

_@akx_

## Summary

Copy both `uv` and `uvx` into place in a single Dockerfile command. [`COPY` supports multiple sources when the destination is a directory.](https://docs.docker.com/engine/reference/builder/#copy)

As it is, e.g. `ghcr.io/astral-sh/uv:0.4.16-python3.12-bookworm-slim` has this (screenshot from [Dive](https://github.com/wagoodman/dive)):

<img width="377" alt="Screenshot 2024-09-26 at 10 11 24" src="https://github.com/user-attachments/assets/1ca6a0d5-95fd-4210-9a4f-0afa2300b63f">

and less layers is a Good Thing.

## Test Plan

I hope the CI pipeline will take care of testing – I couldn't get the Docker build to finish on my machine right away (SIGKILL, so out of memory, I guess 😄)

---

_@konstin approved on 2024-09-26 11:36_

---

_Label `releases` added by @zanieb on 2024-09-26 14:19_

---

_Merged by @zanieb on 2024-09-26 14:19_

---

_Closed by @zanieb on 2024-09-26 14:19_

---

_Branch deleted on 2024-09-26 15:04_

---
