```yaml
number: 1054
title: Wheel download code should respect caching headers
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-23T00:08:45Z
updated_at: 2024-01-24T00:16:09Z
url: https://github.com/astral-sh/uv/issues/1054
synced_at: 2026-01-12T15:58:25Z
```

# Wheel download code should respect caching headers

---

_@charliermarsh_

Right now, we always redownload (or, in reality, never redownload, since we assume wheels are up-to-date in the cache). But URL routes in `get_or_build_wheel` should probably respect caching headers?

---

_Label `bug` added by @charliermarsh on 2024-01-23 00:08_

---

_Comment by @charliermarsh on 2024-01-24 00:16_

Oh, I added this!

---

_Closed by @charliermarsh on 2024-01-24 00:16_

---
