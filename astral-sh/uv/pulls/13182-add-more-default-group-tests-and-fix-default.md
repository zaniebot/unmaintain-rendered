```yaml
number: 13182
title: "Add more default group tests and fix default groups for `uv add`"
type: pull_request
state: merged
author: blueraft
labels:
  - internal
assignees: []
merged: true
base: main
head: tests-default-groups
created_at: 2025-04-28T16:27:17Z
updated_at: 2025-04-29T09:15:44Z
url: https://github.com/astral-sh/uv/pull/13182
synced_at: 2026-01-10T11:10:41Z
```

# Add more default group tests and fix default groups for `uv add`

---

_Pull request opened by @blueraft on 2025-04-28 16:27_

## Summary

Brings in a bug fix for `uv add` w.r.t default groups from https://github.com/astral-sh/uv/pull/12964, see comment: https://github.com/astral-sh/uv/pull/12964#discussion_r2060775131

Adds additional test coverage for default groups in `run`, `remove`, `add`.

## Test Plan

`cargo test`


---

_Review requested from @Gankra by @konstin on 2025-04-28 16:27_

---

_@Gankra approved on 2025-04-28 17:31_

rad!

---

_Comment by @blueraft on 2025-04-28 18:24_

not sure why that test failed, I'll close and re-open

---

_Closed by @blueraft on 2025-04-28 18:24_

---

_Reopened by @blueraft on 2025-04-28 18:24_

---

_Merged by @Gankra on 2025-04-28 19:02_

---

_Closed by @Gankra on 2025-04-28 19:02_

---

_Label `internal` added by @Gankra on 2025-04-28 19:02_

---

_Branch deleted on 2025-04-29 09:15_

---
