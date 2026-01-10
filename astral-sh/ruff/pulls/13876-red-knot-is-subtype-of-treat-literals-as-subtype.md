```yaml
number: 13876
title: "[red-knot] is_subtype_of: treat literals as subtype of 'object'"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/subtype-fixes
created_at: 2024-10-22T10:16:34Z
updated_at: 2024-10-22T11:32:54Z
url: https://github.com/astral-sh/ruff/pull/13876
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] is_subtype_of: treat literals as subtype of 'object'

---

_Pull request opened by @sharkdp on 2024-10-22 10:16_

## Summary

Just something I found while trying to fix #13870. But probably useful on its own.

Add the following subtype relations:
- `BooleanLiteral <: object`
- `IntLiteral <: object`
- `StringLiteral <: object`
- `LiteralString <: object`
- `BytesLiteral <: object`

Added a test case for `bool <: int`.

## Test Plan

New unit tests.


---

_Review requested from @carljm by @sharkdp on 2024-10-22 10:16_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-22 10:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-22 10:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-22 10:21_

Another way of doing this might be to move these branches higher up so that they come immediately below the `Type::Never` branches, before any of the `Literal` branches:

https://github.com/astral-sh/ruff/blob/cd6c9371943db7d10fc5c449eccfb2700c485cd2/crates/red_knot_python_semantic/src/types.rs#L445-L446

Which might be slightly less efficient, but also slightly less error-prone? Since we won't have to remember to account for `object` in all the other branches if we deal with them upfront at the top of the `match`

---

_@AlexWaygood reviewed on 2024-10-22 10:21_

---

_Comment by @github-actions[bot] on 2024-10-22 10:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-10-22 10:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-22 10:31_

That also works, yes. I thought about it.

> Which might be slightly less efficient

That's why I didn't implement it this way. Happy to change it though, if you think it helps. On the other hand, if we add new type variants, we would probably check what other branches do.. but then again, your solution would be less code :man_shrugging: 

---

_@AlexWaygood reviewed on 2024-10-22 10:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-22 10:32_

I don't mind too much; happy to defer to what you think is best. I'm sure you have better instincts than me for what will be best in terms of performance ;)

---

_@AlexWaygood approved on 2024-10-22 10:32_

---

_Label `red-knot` added by @MichaReiser on 2024-10-22 10:44_

---

_@sharkdp reviewed on 2024-10-22 10:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:419 on 2024-10-22 10:56_

Let's not trust any instincts when it comes to performance. I like your version better — changed.

---

_@AlexWaygood approved on 2024-10-22 10:57_

---

_Merged by @sharkdp on 2024-10-22 11:32_

---

_Closed by @sharkdp on 2024-10-22 11:32_

---

_Branch deleted on 2024-10-22 11:32_

---
