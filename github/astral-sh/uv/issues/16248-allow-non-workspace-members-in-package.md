---
number: 16248
title: "Allow non-workspace members in `--package`"
type: issue
state: open
author: charliermarsh
labels:
  - needs-decision
assignees: []
created_at: 2025-10-11T01:40:21Z
updated_at: 2025-10-29T01:06:35Z
url: https://github.com/astral-sh/uv/issues/16248
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow non-workspace members in `--package`

---

_Issue opened by @charliermarsh on 2025-10-11 01:40_

I need to figure out if this desirable and/or what makes it difficult, but it's plausible that `--package` could accept any package, not just workspace members.

I seem to recall that there's some code that relies on entrypoints being unique in the lockfile. It might have to do with conflict markers (if you have multiple `[[package]]` entries for a non-root package, you need to choose between them; and doing so could depend on which parent package would / wouldn't have been included).


---

_Label `needs-decision` added by @charliermarsh on 2025-10-11 01:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-10-29 01:06_

---

_Referenced in [astral-sh/uv#16544](../../astral-sh/uv/pulls/16544.md) on 2025-11-01 02:16_

---
