```yaml
number: 4418
title: "`uv add` should not have to perform a full `uv sync`"
type: issue
state: closed
author: ibraheemdev
labels:
  - preview
assignees: []
created_at: 2024-06-19T18:23:55Z
updated_at: 2024-08-01T20:22:39Z
url: https://github.com/astral-sh/uv/issues/4418
synced_at: 2026-01-12T15:58:50Z
```

# `uv add` should not have to perform a full `uv sync`

---

_@ibraheemdev_

`uv add` currently performs a `uv sync` including dev dependencies and all extras. We should either accept CLI arguments to propagate to `uv sync` or somehow track the state of the environment to know the expected sync behavior.

---

_Label `preview` added by @ibraheemdev on 2024-06-19 18:24_

---

_Closed by @zanieb on 2024-08-01 20:22_

---
