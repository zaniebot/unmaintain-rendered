---
number: 2711
title: Cache issues when upgrading to 0.1.25
type: issue
state: closed
author: hsheth2
labels:
  - bug
assignees: []
created_at: 2024-03-28T16:32:23Z
updated_at: 2024-03-28T17:29:31Z
url: https://github.com/astral-sh/uv/issues/2711
synced_at: 2026-01-07T13:12:17-06:00
---

# Cache issues when upgrading to 0.1.25

---

_Issue opened by @hsheth2 on 2024-03-28 16:32_

With uv 0.1.25

```
$ uv pip install avro
error: Failed to download and build: avro==1.11.3
  Caused by: Failed to deserialize cache entry
  Caused by: expected version to start with a number, but no leading ASCII digits were found
```

With uv 0.1.24, this works fine.

Both commands were run with that avro package in the cache. Not sure if there was a change in the cache format that might've caused this.


---

_Comment by @hsheth2 on 2024-03-28 16:36_

After a `uv cache clean`, things work as expected. So it seems like a change in cache format.

---

_Renamed from "Regression in installing `avro`" to "Cache issues when upgrading to 0.1.25" by @hsheth2 on 2024-03-28 16:36_

---

_Comment by @gibsondan on 2024-03-28 16:56_

Seeing a similar issue in a similar timeframe:

```
[2024-03-28T16:04:14Z] 5.288 error: Failed to download and build: pdpyras==5.2.0
[2024-03-28T16:04:14Z] 5.288   Caused by: Failed to deserialize cache entry
[2024-03-28T16:04:14Z] 5.288   Caused by: expected version to start with a number, but no leading ASCII digits were found
```

---

_Label `bug` added by @zanieb on 2024-03-28 17:06_

---

_Comment by @zanieb on 2024-03-28 17:06_

Thanks for raising. We'll be addressing this immediately.

---

_Referenced in [astral-sh/uv#2712](../../astral-sh/uv/pulls/2712.md) on 2024-03-28 17:18_

---

_Closed by @zanieb on 2024-03-28 17:29_

---

_Referenced in [astral-sh/uv#2714](../../astral-sh/uv/pulls/2714.md) on 2024-03-28 17:37_

---

_Referenced in [astral-sh/uv#2715](../../astral-sh/uv/pulls/2715.md) on 2024-03-28 18:14_

---

_Referenced in [astral-sh/uv#2716](../../astral-sh/uv/issues/2716.md) on 2024-03-28 18:26_

---
