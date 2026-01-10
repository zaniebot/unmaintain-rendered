---
number: 575
title: Store wheels and JSON metadata in separate directories
type: issue
state: closed
author: charliermarsh
labels:
  - performance
  - internal
assignees: []
created_at: 2023-12-06T00:45:51Z
updated_at: 2023-12-07T05:02:48Z
url: https://github.com/astral-sh/uv/issues/575
synced_at: 2026-01-10T01:23:04Z
---

# Store wheels and JSON metadata in separate directories

---

_Issue opened by @charliermarsh on 2023-12-06 00:45_

In the cache. Otherwise, we have to iterate over twice as many files whenever we need to perform a lookup.

---

_Label `performance` added by @charliermarsh on 2023-12-06 00:45_

---

_Label `internal` added by @charliermarsh on 2023-12-06 00:45_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-06 00:45_

---

_Comment by @charliermarsh on 2023-12-06 22:16_

This is less important now that we're sharding the cache.

---

_Referenced in [astral-sh/uv#583](../../astral-sh/uv/pulls/583.md) on 2023-12-06 22:16_

---

_Closed by @charliermarsh on 2023-12-07 05:02_

---
