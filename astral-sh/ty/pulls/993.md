```yaml
number: 993
title: "Port `__main__.py` functionality from uv"
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/port-uv-py
created_at: 2025-08-14T18:47:55Z
updated_at: 2025-08-14T18:49:51Z
url: https://github.com/astral-sh/ty/pull/993
synced_at: 2026-01-10T02:34:10Z
```

# Port `__main__.py` functionality from uv

---

_Pull request opened by @zanieb on 2025-08-14 18:47_

As part of https://github.com/astral-sh/ty/issues/989, but includes some other changes:

- Sets `VIRTUAL_ENV` if `pyvenv.cfg` can be found next to `sys.prefix`
- Sets `TY__PARENT_INTERPRETER` to `sys.executable`
- Elides tracebacks on interrupt on Windows

---

_Comment by @zanieb on 2025-08-14 18:49_

To be honest, I'm not sure what setting `VIRTUAL_ENV` does if we always respect `TY__PARENT_INTERPRETER` over `VIRTUAL_ENV`. That may be cruft in uv.

---
