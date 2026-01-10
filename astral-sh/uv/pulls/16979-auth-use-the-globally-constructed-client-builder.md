---
number: 16979
title: "auth: use the globally constructed client builder"
type: pull_request
state: closed
author: zsol
labels:
  - internal
assignees: []
base: main
head: zsol/jj-rwztvoqowvuy
created_at: 2025-12-04T15:15:27Z
updated_at: 2025-12-04T20:04:36Z
url: https://github.com/astral-sh/uv/pull/16979
synced_at: 2026-01-10T01:26:19Z
---

# auth: use the globally constructed client builder

---

_Pull request opened by @zsol on 2025-12-04 15:15_

## Summary

Instead of each subcommand instantiating its own `BaseClientBuilder`, let's use the globally constructed one.


## Test Plan

Existing tests.


---

_Review requested from @konstin by @zsol on 2025-12-04 15:15_

---

_Review request for @konstin removed by @zsol on 2025-12-04 15:15_

---

_Review requested from @zanieb by @zsol on 2025-12-04 15:15_

---

_Review requested from @konstin by @zsol on 2025-12-04 15:15_

---

_Comment by @zanieb on 2025-12-04 15:17_

I think we should probably retain the settings structs even if they're redundant just for consistency with the rest of the code base.

---

_@zanieb approved on 2025-12-04 15:20_

---

_Label `internal` added by @zanieb on 2025-12-04 15:20_

---

_Referenced in [astral-sh/uv#16886](../../astral-sh/uv/pulls/16886.md) on 2025-12-04 15:20_

---

_Marked ready for review by @zsol on 2025-12-04 19:02_

---

_Merged by @zanieb on 2025-12-04 20:04_

---

_Closed by @zanieb on 2025-12-04 20:04_

---

_Branch deleted on 2025-12-04 20:04_

---
