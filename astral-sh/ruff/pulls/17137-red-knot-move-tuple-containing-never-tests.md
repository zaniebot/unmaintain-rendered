```yaml
number: 17137
title: "[red-knot] Move tuple containing `Never` tests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/move-test
created_at: 2025-04-02T02:35:00Z
updated_at: 2025-04-02T14:27:02Z
url: https://github.com/astral-sh/ruff/pull/17137
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Move tuple containing `Never` tests

---

_@dhruvmanila_

Refer https://github.com/astral-sh/ruff/pull/17094#discussion_r2023682840

---

_Label `internal` added by @dhruvmanila on 2025-04-02 02:35_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-02 02:35_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-02 02:35_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-02 02:35_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-02 02:35_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-02 02:35_

---

_Comment by @github-actions[bot] on 2025-04-02 02:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @dhruvmanila on 2025-04-02 02:46_

---

_Closed by @dhruvmanila on 2025-04-02 02:46_

---

_Branch deleted on 2025-04-02 02:46_

---

_@sharkdp reviewed on 2025-04-02 06:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:26 on 2025-04-02 06:21_

Not that it's particularly important, but do these really test something other than the assertions in lines 16-18?

---

_@AlexWaygood reviewed on 2025-04-02 14:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:26 on 2025-04-02 14:14_

I see your point, but IMO it "says the same thing in a different way", and I think that can be valuable for human readers of the test

---

_@dhruvmanila reviewed on 2025-04-02 14:27_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/tuples_containing_never.md`:26 on 2025-04-02 14:27_

To add to that, I'd say it's testing the "simplified to `Never`" part specifically and more precisely.

---
