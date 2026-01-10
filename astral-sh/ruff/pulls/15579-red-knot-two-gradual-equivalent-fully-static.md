```yaml
number: 15579
title: "[red-knot] Two gradual equivalent fully static types are also equivalent"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: rk-pt-is-gradual-equivalent-to
created_at: 2025-01-19T00:20:35Z
updated_at: 2025-01-19T16:38:47Z
url: https://github.com/astral-sh/ruff/pull/15579
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Two gradual equivalent fully static types are also equivalent

---

_Pull request opened by @InSyncWithFoo on 2025-01-19 00:20_

## Summary

Follow-up to #15563, related to [this comment](https://github.com/astral-sh/ruff/pull/15563#discussion_r1921058845) there.

## Test Plan

Type property tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-19 00:20_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-19 00:20_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-19 00:20_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-19 00:20_

---

_Comment by @github-actions[bot] on 2025-01-19 00:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @AlexWaygood on 2025-01-19 02:39_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-19 02:39_

---

_@AlexWaygood reviewed on 2025-01-19 16:19_

Thanks. Can you say a little bit about why you added this test to the `flaky` submodule in this file rather than the `stable` submodule? Did it ever fail when you ran the property tests locally? I stress-tested the flaky property tests by running `QUICKCHECK_TESTS=2000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::flaky` several times locally on this branch (after merging in `main`), and neither the new test you're adding here nor `two_equivalent_types_are_also_gradual_equivalent` failed once.

That implies to me that we should be adding this test to the `stable` submodule in this file? And also promoting `two_equivalent_types_are_also_gradual_equivalent` to stable?

---

_Comment by @InSyncWithFoo on 2025-01-19 16:33_

Promoted. I thought I should remain careful "just in case".

---

_Merged by @AlexWaygood on 2025-01-19 16:37_

---

_Closed by @AlexWaygood on 2025-01-19 16:37_

---

_Branch deleted on 2025-01-19 16:37_

---
