---
number: 6658
title: Warn on invalid script target module names
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-08-26T20:38:14Z
updated_at: 2024-08-26T20:38:57Z
url: https://github.com/astral-sh/uv/issues/6658
synced_at: 2026-01-10T01:24:04Z
---

# Warn on invalid script target module names

---

_Issue opened by @zanieb on 2024-08-26 20:38_

e.g. in an new `uv init` project

```
[project]
name = "uv-docker-example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project.scripts]
hello = "uv-docker-example:hello"
```

`uv run hello` errors with

```
  File "/Users/zb/workspace/uv-docker-example/.venv/bin/hello", line 5
    from uv-docker-example import hello
           ^
SyntaxError: invalid syntax
```

which was very confusing, but was caused by the invalid module name in `project.scripts.hello`. We should warn when we see this.

---

_Label `error messages` added by @zanieb on 2024-08-26 20:38_

---
