```yaml
number: 6922
title: "Allow `Locator#slice` to take `Ranged`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/slice
created_at: 2023-08-27T19:14:39Z
updated_at: 2023-08-28T15:08:40Z
url: https://github.com/astral-sh/ruff/pull/6922
synced_at: 2026-01-12T15:55:22Z
```

# Allow `Locator#slice` to take `Ranged`

---

_@charliermarsh_

## Summary

As a small quality-of-life improvement, the locator can now slice like `locator.slice(stmt)` instead of requiring `locator.slice(stmt.range())`.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-27 19:20_

---

_Label `internal` added by @charliermarsh on 2023-08-27 19:20_

---

_Comment by @github-actions[bot] on 2023-08-27 19:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser approved on 2023-08-28 06:12_

---

_Merged by @charliermarsh on 2023-08-28 15:08_

---

_Closed by @charliermarsh on 2023-08-28 15:08_

---

_Branch deleted on 2023-08-28 15:08_

---
