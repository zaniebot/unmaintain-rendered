```yaml
number: 12910
title: "Require `--force` to replace existing virtual environments in `uv venv`"
type: pull_request
state: open
author: zanieb
labels:
  - breaking
assignees: []
draft: true
base: main
head: zb/venv-force
created_at: 2025-04-15T21:52:29Z
updated_at: 2025-04-16T03:55:08Z
url: https://github.com/astral-sh/uv/pull/12910
synced_at: 2026-01-12T16:10:26Z
```

# Require `--force` to replace existing virtual environments in `uv venv`

---

_@zanieb_

There are a few glaring problems with this, notably that `-f` is taken by `--find-links` (of all things) and that the error formatting is weird. I've posted it regardless to push forward discussion on https://github.com/astral-sh/uv/issues/1472 with a tangible user experience suggestion.

---

_Label `breaking` added by @zanieb on 2025-04-15 21:52_

---
