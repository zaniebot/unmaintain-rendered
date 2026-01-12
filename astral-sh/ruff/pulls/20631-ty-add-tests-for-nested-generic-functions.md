```yaml
number: 20631
title: "[ty] Add tests for nested generic functions"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/nested-generics-tests
created_at: 2025-09-29T14:35:50Z
updated_at: 2025-09-30T06:44:20Z
url: https://github.com/astral-sh/ruff/pull/20631
synced_at: 2026-01-12T15:57:06Z
```

# [ty] Add tests for nested generic functions

---

_@sharkdp_

## Summary

Add two simple tests that we recently discussed with @dcreager. They demonstrate that the `TypeMapping::MarkTypeVarsInferable` operation really does need to keep track of the binding context.

## Test Plan

Made sure that those tests fail if we create `TypeMapping::MarkTypeVarsInferable(None)`s everywhere.

---

_Review requested from @carljm by @sharkdp on 2025-09-29 14:35_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-29 14:35_

---

_Review requested from @dcreager by @sharkdp on 2025-09-29 14:35_

---

_Label `testing` added by @sharkdp on 2025-09-29 14:35_

---

_Label `ty` added by @sharkdp on 2025-09-29 14:35_

---

_@AlexWaygood approved on 2025-09-29 14:54_

---

_Merged by @sharkdp on 2025-09-30 06:44_

---

_Closed by @sharkdp on 2025-09-30 06:44_

---

_Branch deleted on 2025-09-30 06:44_

---
