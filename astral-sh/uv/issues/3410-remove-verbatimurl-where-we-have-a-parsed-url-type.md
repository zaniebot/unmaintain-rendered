---
number: 3410
title: "Remove `VerbatimUrl` where we have a parsed url type"
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-05-06T14:20:30Z
updated_at: 2024-11-20T00:32:30Z
url: https://github.com/astral-sh/uv/issues/3410
synced_at: 2026-01-10T01:23:27Z
---

# Remove `VerbatimUrl` where we have a parsed url type

---

_Issue opened by @konstin on 2024-05-06 14:20_

Final item for #3407: Cleanup.

* Remove the `VerbatimUrl` in the `VerbatimParsedUrl`, store only `given`.
  * Change distribution and package ids
  * Adapt canonical url handling
  * Change cache key handling
* Check if we can use some abstraction so we don't copy all fields explicitly each time and have common naming.

---

_Label `enhancement` added by @konstin on 2024-05-06 14:20_

---

_Assigned to @konstin by @konstin on 2024-05-06 14:20_

---

_Referenced in [astral-sh/uv#3407](../../astral-sh/uv/issues/3407.md) on 2024-05-06 14:21_

---

_Referenced in [astral-sh/uv#3758](../../astral-sh/uv/pulls/3758.md) on 2024-05-23 21:01_

---

_Unassigned @konstin by @konstin on 2024-06-20 09:07_

---

_Closed by @charliermarsh on 2024-11-20 00:32_

---
