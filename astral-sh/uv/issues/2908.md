```yaml
number: 2908
title: Cache is not robust to moves
type: issue
state: closed
author: charliermarsh
labels:
  - wish
assignees: []
created_at: 2024-04-08T19:12:24Z
updated_at: 2024-04-11T01:07:53Z
url: https://github.com/astral-sh/uv/issues/2908
synced_at: 2026-01-10T05:40:32Z
```

# Cache is not robust to moves

---

_Issue opened by @charliermarsh on 2024-04-08 19:12_

If you use `--cache-dir foo`, then `mv foo bar`, and then `--cache-dir bar`, commands will fail, since we write absolute paths to various places in the cache.


---

_Label `bug` added by @charliermarsh on 2024-04-08 19:12_

---

_Label `wish` added by @charliermarsh on 2024-04-08 19:12_

---

_Label `bug` removed by @charliermarsh on 2024-04-08 19:12_

---

_Comment by @charliermarsh on 2024-04-08 19:12_

Not a bug per se, but would be a nice limitation to lift.

---

_Comment by @charliermarsh on 2024-04-10 19:18_

I may try to fix this before releasing hash-checking, so it can go out in the same version bump.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 00:44_

---

_Closed by @charliermarsh on 2024-04-11 01:07_

---
