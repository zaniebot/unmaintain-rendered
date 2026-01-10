---
number: 480
title: Use index as cache shard
type: issue
state: closed
author: konstin
labels:
  - wish
  - internal
assignees: []
created_at: 2023-11-21T16:15:49Z
updated_at: 2024-01-11T16:54:26Z
url: https://github.com/astral-sh/uv/issues/480
synced_at: 2026-01-10T01:23:04Z
---

# Use index as cache shard

---

_Issue opened by @konstin on 2023-11-21 16:15_

We should flip the current cache structure and use the index as cache shard, or `url/<digest(url)>`/`git/<digest(repo_url)>`/`path/<digest(path)>` where no index is used. The process of doing this ensures that we properly separate by index (which we currently don't) and it allow clearing a single index from the cache.

---

_Referenced in [astral-sh/uv#481](../../astral-sh/uv/issues/481.md) on 2023-11-21 16:17_

---

_Comment by @charliermarsh on 2023-11-22 14:59_

Can you expand on this one? Is this not part of your recent refactor? I thought we were using `sha(index)` as the shard.

---

_Comment by @konstin on 2023-11-22 15:01_

Current we have `<bucket_kind>/<digest(index)>`, i want to change it to  `<digest(index)>/<bucket_kind>`. This doesn't have really have a user facing effect, it's rather low priority.

---

_Label `wish` added by @charliermarsh on 2023-11-25 17:25_

---

_Label `internal` added by @charliermarsh on 2023-11-25 17:25_

---

_Comment by @konstin on 2024-01-11 16:54_

I think the current one works fine

---

_Closed by @konstin on 2024-01-11 16:54_

---
