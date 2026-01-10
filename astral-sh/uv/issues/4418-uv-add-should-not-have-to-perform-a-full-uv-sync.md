---
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
synced_at: 2026-01-10T01:23:37Z
---

# `uv add` should not have to perform a full `uv sync`

---

_Issue opened by @ibraheemdev on 2024-06-19 18:23_

`uv add` currently performs a `uv sync` including dev dependencies and all extras. We should either accept CLI arguments to propagate to `uv sync` or somehow track the state of the environment to know the expected sync behavior.

---

_Label `preview` added by @ibraheemdev on 2024-06-19 18:24_

---

_Referenced in [astral-sh/uv#3959](../../astral-sh/uv/issues/3959.md) on 2024-06-19 18:25_

---

_Referenced in [astral-sh/uv#5705](../../astral-sh/uv/pulls/5705.md) on 2024-08-01 19:02_

---

_Closed by @zanieb on 2024-08-01 20:22_

---

_Referenced in [treyhunner/uvrs#9](../../treyhunner/uvrs/issues/9.md) on 2025-10-16 22:54_

---
