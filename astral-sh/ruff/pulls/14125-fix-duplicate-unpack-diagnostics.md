```yaml
number: 14125
title: Fix duplicate unpack diagnostics
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-unpack-duplicate-diagnostics
created_at: 2024-11-06T09:06:10Z
updated_at: 2024-11-06T11:37:38Z
url: https://github.com/astral-sh/ruff/pull/14125
synced_at: 2026-01-10T20:50:57Z
```

# Fix duplicate unpack diagnostics

---

_Pull request opened by @MichaReiser on 2024-11-06 09:06_

## Summary

Fixes the issue where we emitted duplicated diagnostics for unpack assignments. 

The solution is fairly simple. All we need is a deterministic way to know when we should add the
diagnostics. The approach I used here is only to add the diagnostics for the first unpacking.

This works because `check_types` pulls all types. 

What about accumulators. We may still want to use accumulators in the future, e.g. if other, non type checking queries start emitting diagnostics. However, we have to solve the performance regression first. I have an idea on how that could be done. See the [zulip discussion](https://salsa.zulipchat.com/#narrow/channel/333573-Contributing-to-Salsa/topic/Accumulator.20performance)

## Test Plan

See updated mdtest


---

_Review requested from @carljm by @MichaReiser on 2024-11-06 09:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-06 09:06_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-06 09:06_

---

_Label `red-knot` added by @MichaReiser on 2024-11-06 09:06_

---

_Comment by @github-actions[bot] on 2024-11-06 09:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:148 on 2024-11-06 10:54_

Should fix the mdformat complaints in pre-commit:

```suggestion
```

---

_@AlexWaygood approved on 2024-11-06 10:55_

Nice!

---

_@dhruvmanila approved on 2024-11-06 11:05_

---

_Merged by @MichaReiser on 2024-11-06 11:28_

---

_Closed by @MichaReiser on 2024-11-06 11:28_

---

_Branch deleted on 2024-11-06 11:28_

---
