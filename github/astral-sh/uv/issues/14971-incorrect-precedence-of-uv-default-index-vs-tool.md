---
number: 14971
title: Incorrect precedence of UV_DEFAULT_INDEX vs tool.uv.index-url
type: issue
state: closed
author: gab23r
labels:
  - question
assignees: []
created_at: 2025-07-30T13:45:19Z
updated_at: 2025-07-31T02:03:19Z
url: https://github.com/astral-sh/uv/issues/14971
synced_at: 2026-01-07T13:12:19-06:00
---

# Incorrect precedence of UV_DEFAULT_INDEX vs tool.uv.index-url

---

_Issue opened by @gab23r on 2025-07-30 13:45_

### Summary

UV_DEFAULT_INDEX should be ignored if tool.uv.index-url is given in the pyproject.toml.

Today, this is not the case:
```
# pyproject.toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "ruff>=0.12.7",
]

[tool.uv]
index-url = "https://example.org/"
```


```
UV_DEFAULT_INDEX=https://pypi.org/simple uv sync
```

This should raise: `Caused by: Failed to fetch: `https://example.org/ruff/``

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

3.13

---

_Label `bug` added by @gab23r on 2025-07-30 13:45_

---

_Renamed from "Incorrect precedence of UV_INDEX_URL vs tool.uv.index-url" to "Incorrect precedence of UV_DEFAULT_INDEX vs tool.uv.index-url" by @gab23r on 2025-07-30 13:53_

---

_Comment by @zanieb on 2025-07-30 14:41_

Environment variables take precedence over configuration files, like the `pyproject.toml`.

---

_Label `bug` removed by @zanieb on 2025-07-30 14:41_

---

_Label `question` added by @zanieb on 2025-07-30 14:41_

---

_Closed by @charliermarsh on 2025-07-31 02:03_

---
