---
number: 5139
title: "`uv tool run` always picks up older python `3.8` over `3.12`"
type: issue
state: closed
author: j178
labels: []
assignees: []
created_at: 2024-07-17T03:52:34Z
updated_at: 2024-07-17T14:48:05Z
url: https://github.com/astral-sh/uv/issues/5139
synced_at: 2026-01-10T01:23:45Z
---

# `uv tool run` always picks up older python `3.8` over `3.12`

---

_Issue opened by @j178 on 2024-07-17 03:52_

```sh
$ uv python install 3.8 3.12

$ uv tool run -v httpx
DEBUG uv 0.2.25
warning: `uv tool run` is experimental and may change without warning.
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/didi/Library/Application Support/uv/python`
DEBUG Found managed Python `cpython-3.8.19-macos-x86_64-none`
DEBUG Found cpython 3.8.19 at `/Users/didi/Library/Application Support/uv/python/cpython-3.8.19-macos-x86_64-none/bin/python3` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/Users/didi/Library/Application Support/uv/tools`
DEBUG Caching via interpreter: `/Users/didi/Library/Application Support/uv/python/cpython-3.8.19-macos-x86_64-none/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.8.19
DEBUG Adding direct dependency: httpx*
...
```

---

_Referenced in [astral-sh/uv#5140](../../astral-sh/uv/pulls/5140.md) on 2024-07-17 03:55_

---

_Closed by @zanieb on 2024-07-17 14:48_

---

_Referenced in [astral-sh/uv#5205](../../astral-sh/uv/pulls/5205.md) on 2024-07-19 01:14_

---
