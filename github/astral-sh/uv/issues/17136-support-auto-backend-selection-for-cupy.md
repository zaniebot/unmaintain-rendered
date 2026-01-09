---
number: 17136
title: Support auto backend selection for CuPy
type: issue
state: open
author: MattTheCuber
labels:
  - enhancement
assignees: []
created_at: 2025-12-15T17:04:16Z
updated_at: 2025-12-15T17:04:16Z
url: https://github.com/astral-sh/uv/issues/17136
synced_at: 2026-01-07T13:12:19-06:00
---

# Support auto backend selection for CuPy

---

_Issue opened by @MattTheCuber on 2025-12-15 17:04_

### Summary

CuPy installation offers specific backend selection to speed up the installation process. For example, you need to use `uv pip install cupy-cuda13x` on a CUDA 13 system (see the docs [here](https://docs.cupy.dev/en/stable/install.html)).

The UV documentation describes using `--torch-backend=auto`, it would be nice if this would also apply to CuPy.

### Example

`uv pip install cupy --backend=auto`

---

_Label `enhancement` added by @MattTheCuber on 2025-12-15 17:04_

---
