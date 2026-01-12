```yaml
number: 12301
title: Use resolver-returned wheel over alternate cached wheel
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/n
created_at: 2025-03-18T22:16:00Z
updated_at: 2025-03-19T01:41:06Z
url: https://github.com/astral-sh/uv/pull/12301
synced_at: 2026-01-12T16:10:12Z
```

# Use resolver-returned wheel over alternate cached wheel

---

_@charliermarsh_

## Summary

I think this is reasonable to change. Right now, if you're on Python 3.11, the resolver returns `multiprocess-0.70.17-py311-none-any.whl`, but `multiprocess-0.70.17-py310-none-any.whl` is in the cache, we'll reuse `multiprocess-0.70.17-py310-none-any.whl` (since it _is_ compatible with Python 3.11).

Instead, we now _require_ the cached wheel to match the wheel returned by the resolver.

Closes https://github.com/astral-sh/uv/issues/12273.


---

_Label `bug` added by @charliermarsh on 2025-03-18 22:16_

---

_Label `cache` added by @charliermarsh on 2025-03-18 22:16_

---

_Marked ready for review by @charliermarsh on 2025-03-18 22:16_

---

_@zanieb approved on 2025-03-19 00:16_

---

_Merged by @charliermarsh on 2025-03-19 01:41_

---

_Closed by @charliermarsh on 2025-03-19 01:41_

---

_Branch deleted on 2025-03-19 01:41_

---
