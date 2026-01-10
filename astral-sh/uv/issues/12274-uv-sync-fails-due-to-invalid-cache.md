```yaml
number: 12274
title: uv sync fails due to invalid cache
type: issue
state: closed
author: vlaci
labels:
  - bug
assignees: []
created_at: 2025-03-18T11:52:45Z
updated_at: 2025-03-18T15:27:22Z
url: https://github.com/astral-sh/uv/issues/12274
synced_at: 2026-01-10T03:50:31Z
```

# uv sync fails due to invalid cache

---

_Issue opened by @vlaci on 2025-03-18 11:52_

### Summary

It started happening right after upgrading from 0.6.5 to 0.6.6. 

In our environment, `~/.cache/uv` is cached between CI runs. My theory is that for some time uv 0.6.5 ran alternating with 0.6.6 on different branches sharing the same cache, as the issue did not happen when 0.6.6 was first introduced.

The error itself is very similar to the one fixed in https://github.com/astral-sh/uv/pull/11105, but it comes from a different place:

```
$ uv sync --frozen --no-dev --extra xyz
error: Failed to determine installation plan
Caused by: Failed to deserialize cache entry
Caused by: array had incorrect length, expected 4
```

After a cursory glance at the code of `Planner.build()`, it looks like caching didn't get the same treatment in `{Http,Local}ArchivePointer::read_from()` as `SourceDistributionBuilder::url_metadata()` got in https://github.com/astral-sh/uv/pull/11105

### Platform

Debian 12 (bookworm)

### Version

uv 0.6.6

### Python version

CPython 3.11.2

---

_Label `bug` added by @vlaci on 2025-03-18 11:52_

---

_Comment by @charliermarsh on 2025-03-18 13:47_

Tagging @Gankra 

---

_Assigned to @Gankra by @charliermarsh on 2025-03-18 13:47_

---

_Closed by @Gankra on 2025-03-18 15:27_

---

_Closed by @Gankra on 2025-03-18 15:27_

---
