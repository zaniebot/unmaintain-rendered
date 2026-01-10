```yaml
number: 19974
title: "[ty] Add more regression tests for `tuple`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-regression-tests
created_at: 2025-08-18T17:26:00Z
updated_at: 2025-08-18T17:30:07Z
url: https://github.com/astral-sh/ruff/pull/19974
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add more regression tests for `tuple`

---

_Pull request opened by @AlexWaygood on 2025-08-18 17:26_

## Summary

Early versions of #19920 (which we decided not to merge, at least for now) caused incorrect behaviour or panics for both of the situations being tested here, without any tests failing. This indicates that we have missing coverage -- so this PR adds the tests from that branch.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-08-18 17:26_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-18 17:26_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-18 17:26_

---

_Label `testing` added by @AlexWaygood on 2025-08-18 17:26_

---

_Label `ty` added by @AlexWaygood on 2025-08-18 17:26_

---

_Merged by @AlexWaygood on 2025-08-18 17:30_

---

_Closed by @AlexWaygood on 2025-08-18 17:30_

---

_Branch deleted on 2025-08-18 17:30_

---
