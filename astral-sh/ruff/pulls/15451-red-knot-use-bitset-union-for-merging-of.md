```yaml
number: 15451
title: "[red-knot] Use `BitSet::union` for merging of declarations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/use-bitset-union-for-declarations
created_at: 2025-01-13T08:10:17Z
updated_at: 2025-01-13T13:03:56Z
url: https://github.com/astral-sh/ruff/pull/15451
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Use `BitSet::union` for merging of declarations

---

_@sharkdp_

## Summary

In `SymbolState` merging, use `BitSet::union` instead of inserting declarations one by one. This used to be the case but was changed in https://github.com/astral-sh/ruff/pull/15019 because we had to iterate over declarations anyway.

This is an alternative to https://github.com/astral-sh/ruff/pull/15419 by @MichaReiser, just to see which is better in terms of performance.

## Codspeed results

There's almost no difference in performance (note that the baseline also changed):

![image](https://github.com/user-attachments/assets/6e128a1b-c3b4-4963-914b-9a564cac90cc)

![image](https://github.com/user-attachments/assets/b723b29e-cea4-40c7-9f5f-769d84b5a78c)



## Test Plan

—

---

_Label `red-knot` added by @sharkdp on 2025-01-13 08:10_

---

_Review requested from @carljm by @sharkdp on 2025-01-13 08:10_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-13 08:10_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-13 08:10_

---

_Comment by @github-actions[bot] on 2025-01-13 08:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-13 08:28_

Thanks. This is the proper solution which I think is also easier to understand (it's clear that the order isn't relevant). 

---

_Merged by @sharkdp on 2025-01-13 10:10_

---

_Closed by @sharkdp on 2025-01-13 10:10_

---

_Branch deleted on 2025-01-13 10:10_

---

_@AlexWaygood reviewed on 2025-01-13 12:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:97 on 2025-01-13 12:18_

nit that is probably not even worth a followup PR: I'd have probably called this something like `union_with` (similar to https://docs.rs/bit-set/latest/bit_set/struct.BitSet.html#method.union_with) since `union` methods on other structs seem to generally return a new value rather than working in-place (https://doc.rust-lang.org/std/collections/struct.HashSet.html#method.union, https://docs.rs/bitflags/latest/bitflags/trait.Flags.html#method.union, https://docs.rs/bit-set/latest/bit_set/struct.BitSet.html#method.union)

---

_@sharkdp reviewed on 2025-01-13 13:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:97 on 2025-01-13 13:03_

Thanks. I just put some previously-approved code back in place that was there before, which is why I didn't wait for any other reviews. But that seems like a reasonable improvement.

---
