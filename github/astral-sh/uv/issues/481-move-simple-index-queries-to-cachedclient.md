---
number: 481
title: "Move simple index queries to `CachedClient`"
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2023-11-21T16:17:19Z
updated_at: 2023-11-28T00:11:06Z
url: https://github.com/astral-sh/uv/issues/481
synced_at: 2026-01-07T13:12:16-06:00
---

# Move simple index queries to `CachedClient`

---

_Issue opened by @konstin on 2023-11-21 16:17_

Currently simple index queries use the [http_cache](https://docs.rs/http-cache/latest/http_cache). We should move them to `CachedClient`. This allows merging different types of index response before caching, sharding by index (#480), saving a smaller response (we can filter it down to wheels, source dists and sentinel versions) and makes optimizations for much faster cache reading and writing possible.

---

_Label `performance` added by @charliermarsh on 2023-11-25 17:25_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-25 17:26_

---

_Referenced in [astral-sh/uv#504](../../astral-sh/uv/pulls/504.md) on 2023-11-27 14:00_

---

_Closed by @charliermarsh on 2023-11-28 00:11_

---
