---
number: 477
title: Fix wheel caching
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-21T16:06:58Z
updated_at: 2023-12-07T13:03:35Z
url: https://github.com/astral-sh/uv/issues/477
synced_at: 2026-01-07T13:12:16-06:00
---

# Fix wheel caching

---

_Issue opened by @konstin on 2023-11-21 16:06_

- [x] Unzipped wheels should use the same structure as their metadata as introduced in #462. Currently, we drop information about the index, the url and the wheel tag, which leads to collisions and installing incompatible wheels
- [x] Replace the `CacheShard` with `WheelMetadataCache` which handles urls properly.
- [x] Delete unzipped wheels when their remote wheel changed
- [x] Store built wheels next to the `metadata.json` in the source dist directory; delete built wheels when their source dist changed (different cache bucket, but it's the same problem of fixing wheel caching)

---

_Assigned to @konstin by @konstin on 2023-11-21 16:07_

---

_Referenced in [astral-sh/uv#377](../../astral-sh/uv/issues/377.md) on 2023-11-21 16:22_

---

_Added to milestone `Feature complete` by @konstin on 2023-11-21 16:22_

---

_Label `bug` added by @konstin on 2023-11-21 16:22_

---

_Renamed from "Fix unzipped wheel caching" to "Fix wheel caching" by @konstin on 2023-11-26 15:16_

---

_Referenced in [astral-sh/uv#501](../../astral-sh/uv/pulls/501.md) on 2023-11-26 15:56_

---

_Comment by @charliermarsh on 2023-12-06 01:31_

@konstin - I think some of the items here are done.

---

_Closed by @konstin on 2023-12-07 13:03_

---
