---
number: 478
title: Generic file metadata based caching
type: issue
state: closed
author: konstin
labels:
  - wish
  - internal
assignees: []
created_at: 2023-11-21T16:08:34Z
updated_at: 2023-12-06T16:47:02Z
url: https://github.com/astral-sh/uv/issues/478
synced_at: 2026-01-07T13:12:16-06:00
---

# Generic file metadata based caching

---

_Issue opened by @konstin on 2023-11-21 16:08_

Both the interpreter info as well as path deps need to cache while using the file modification timestamp for cache invalidation. We should abstract this into a shared util, like we do with the `CachedClient`

---

_Label `internal` added by @charliermarsh on 2023-11-25 17:24_

---

_Label `wish` added by @charliermarsh on 2023-11-25 17:25_

---

_Referenced in [astral-sh/uv#508](../../astral-sh/uv/pulls/508.md) on 2023-11-28 16:36_

---

_Referenced in [astral-sh/uv#519](../../astral-sh/uv/pulls/519.md) on 2023-11-30 11:36_

---

_Referenced in [astral-sh/uv#578](../../astral-sh/uv/pulls/578.md) on 2023-12-06 16:32_

---

_Closed by @charliermarsh on 2023-12-06 16:47_

---
