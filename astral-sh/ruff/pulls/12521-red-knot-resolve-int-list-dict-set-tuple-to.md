```yaml
number: 12521
title: "[red-knot] resolve int/list/dict/set/tuple to builtin type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/some-builtins
created_at: 2024-07-26T03:13:42Z
updated_at: 2024-08-02T17:02:29Z
url: https://github.com/astral-sh/ruff/pull/12521
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] resolve int/list/dict/set/tuple to builtin type

---

_Pull request opened by @carljm on 2024-07-26 03:13_

Now that we have builtins available, resolve some simple cases to the right builtin type.

We should also adjust the display for types to include their module name; that's not done yet here.


---

_Review requested from @MichaReiser by @carljm on 2024-07-26 03:13_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-26 03:13_

---

_Label `red-knot` added by @carljm on 2024-07-26 03:13_

---

_Comment by @github-actions[bot] on 2024-07-26 03:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:924 on 2024-07-26 06:05_

Does that mean that type narrowing with big int literals won't be supported because they always infer to `int`?

Should we use the parsers `Int` type?

---

_@MichaReiser approved on 2024-07-26 06:07_

LGTM. It would be great if we could extend our benchmark with some more real world code soon. Maybe as a goal for next week? Just so that we know what the "cost" of the changes

---

_@carljm reviewed on 2024-07-26 15:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:924 on 2024-07-26 15:19_

I think we discussed this when I added the `IntLiteral` type. We probably can use the parser's `Int` type to support big integers. I think I even tried doing that, but ran into some complexities (I just don't remember what they were now.)

This only impacts integer literals outside the i64 range. In my experience these are extremely uncommon (2^63 is a really large number, as you can see in the massive integer literal I added in the test here.) So I'm not sure it's really worth adding complexity or cost to the implementation to support them. Tracking exact literal values of integer types is a best-effort feature anyway; there are many cases where we'll just have to fallback to "it's an int." But we can see if it causes anyone problems in practice.

---

_Merged by @carljm on 2024-07-26 15:21_

---

_Closed by @carljm on 2024-07-26 15:21_

---

_Branch deleted on 2024-07-26 15:21_

---
