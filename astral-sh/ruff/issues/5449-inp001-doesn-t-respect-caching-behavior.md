```yaml
number: 5449
title: "`INP001` doesn't respect caching behavior"
type: issue
state: open
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
created_at: 2023-06-30T02:18:34Z
updated_at: 2024-09-05T06:31:25Z
url: https://github.com/astral-sh/ruff/issues/5449
synced_at: 2026-01-10T11:09:47Z
```

# `INP001` doesn't respect caching behavior

---

_Issue opened by @charliermarsh on 2023-06-30 02:18_

See #2075. It looks like the underlying issue (that we don't invalidate the cache when an `__init__.py` is added or removed) is back with the latest cache updates.

_Originally posted by @matthias-roussel-soyhuce in https://github.com/astral-sh/ruff/issues/2075#issuecomment-1612618794_


---

_Label `bug` added by @charliermarsh on 2023-06-30 02:19_

---

_Assigned to @zanieb by @zanieb on 2023-07-14 20:47_

---

_Label `cache` added by @MichaReiser on 2024-09-05 06:31_

---
