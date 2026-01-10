```yaml
number: 11285
title: "Treat `-p` in `uv pip compile` as `--python` and fallback to `--python-version`"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2025-02-06T16:48:29Z
updated_at: 2025-02-13T22:17:54Z
url: https://github.com/astral-sh/uv/issues/11285
synced_at: 2026-01-10T03:50:31Z
```

# Treat `-p` in `uv pip compile` as `--python` and fallback to `--python-version`

---

_Issue opened by @zanieb on 2025-02-06 16:48_

Right now, `-p` in `uv pip compile` is an alias for `--python-version` instead of `--python` which means it does not accept things like `pypy@3.10` — this is different than the entire rest of our CLI and is blocking things like https://github.com/astral-sh/uv/pull/3889



---

_Assigned to @zanieb by @zanieb on 2025-02-06 16:48_

---

_Label `cli` added by @zanieb on 2025-02-06 16:48_

---

_Closed by @zanieb on 2025-02-13 22:17_

---
