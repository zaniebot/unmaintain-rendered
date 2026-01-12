```yaml
number: 14436
title: "[red-knot] Do not panic on f-string format spec expressions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/f-string-format-spec-no-panic
created_at: 2024-11-18T15:28:02Z
updated_at: 2024-11-19T09:04:56Z
url: https://github.com/astral-sh/ruff/pull/14436
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Do not panic on f-string format spec expressions

---

_@sharkdp_

## Summary

Previously, we panicked on expressions like `f"{v:{f'0.2f'}}"` because we did not infer types for expressions nested inside format spec elements.

## Test Plan

```
cargo nextest run -p red_knot_workspace -- --ignored linter_af linter_gz
```

---

_Label `red-knot` added by @sharkdp on 2024-11-18 15:28_

---

_Review requested from @carljm by @sharkdp on 2024-11-18 15:28_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-18 15:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-18 15:28_

---

_Comment by @github-actions[bot] on 2024-11-18 15:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2292 on 2024-11-18 15:53_

```suggestion
                                if conversion.is_some() {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2293 on 2024-11-18 15:53_

Isn't it necessary to infer the expression type if `conversion` is some, e.g for `f"{10!s:abcd}`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2300 on 2024-11-18 15:54_

I believe there's a helper method `elements.expressions()` 

---

_@MichaReiser reviewed on 2024-11-18 15:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2292 on 2024-11-18 18:32_

`conversion` is a `ConversionFlag` enum with a `None` element, not an `Option`

---

_@sharkdp reviewed on 2024-11-18 18:32_

---

_@sharkdp reviewed on 2024-11-18 18:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2293 on 2024-11-18 18:36_

In that case, `conversion` will change to `ConversionFlag::Str`, but this enum has no subexpressions or similar. Not that I'm not adding proper type inference support for these f-strings here, I'm merely trying to make sure we infer types for all subexpressions, so that we do not panic. So it's possible we need to look at this flag once we implement this properly, but I don't see why it would be needed now.

---

_@sharkdp reviewed on 2024-11-18 18:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2300 on 2024-11-18 18:38_

Ah nice, thanks.

---

_@MichaReiser reviewed on 2024-11-18 20:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2293 on 2024-11-18 20:11_

What I understand is that we only take the branch inferring the expression types if the conversion is none. What if it isn't?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2293 on 2024-11-19 08:50_

You are totally right. Fixed & added a proper test.

---

_@sharkdp reviewed on 2024-11-19 08:50_

---

_Merged by @sharkdp on 2024-11-19 09:04_

---

_Closed by @sharkdp on 2024-11-19 09:04_

---

_Branch deleted on 2024-11-19 09:04_

---
