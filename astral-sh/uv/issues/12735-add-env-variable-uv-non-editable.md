---
number: 12735
title: Add env variable UV_NON_EDITABLE
type: issue
state: closed
author: haarisr
labels:
  - enhancement
  - good first issue
  - configuration
assignees: []
created_at: 2025-04-08T07:30:13Z
updated_at: 2025-04-10T19:56:08Z
url: https://github.com/astral-sh/uv/issues/12735
synced_at: 2026-01-10T01:25:24Z
---

# Add env variable UV_NON_EDITABLE

---

_Issue opened by @haarisr on 2025-04-08 07:30_

### Summary

Add a env variable for non-editable builds.

It will be useful to set a global default for non-editable builds in docker containers, so that `uv run` and `uv sync` will be non-editable by default for project and workspace members

### Example

_No response_

---

_Label `enhancement` added by @haarisr on 2025-04-08 07:30_

---

_Label `configuration` added by @zanieb on 2025-04-08 18:01_

---

_Label `good first issue` added by @zanieb on 2025-04-08 18:01_

---

_Comment by @zanieb on 2025-04-08 18:01_

I think `UV_NO_EDITABLE` would be the proper name, or `UV_SYNC_NO_EDITABLE`?

---

_Comment by @charliermarsh on 2025-04-08 19:44_

Yeah `UV_NO_EDITABLE` 👍 

---

_Referenced in [astral-sh/uv#12773](../../astral-sh/uv/pulls/12773.md) on 2025-04-09 07:15_

---

_Closed by @Gankra on 2025-04-10 19:56_

---

_Closed by @Gankra on 2025-04-10 19:56_

---
