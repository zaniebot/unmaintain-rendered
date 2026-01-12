```yaml
number: 20646
title: "[ty] Literal promotion refactor"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/promote-literals-refactor
created_at: 2025-09-30T09:54:21Z
updated_at: 2025-09-30T12:22:38Z
url: https://github.com/astral-sh/ruff/pull/20646
synced_at: 2026-01-12T15:57:06Z
```

# [ty] Literal promotion refactor

---

_@sharkdp_

## Summary

Not sure if this was the original intention, but it looks to me like the previous `Type::literal_promotion_type` was more of an implementation detail for the actual operation of promoting all literals in a possibly-nested position of a type.

This is not a pure refactor, as I'm technically changing the behavior for that protocols diagnostic message suggestion.

## Test Plan

New Markdown test

---

_Review requested from @carljm by @sharkdp on 2025-09-30 09:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-30 09:54_

---

_Review requested from @dcreager by @sharkdp on 2025-09-30 09:54_

---

_Label `ty` added by @sharkdp on 2025-09-30 09:54_

---

_@sharkdp reviewed on 2025-09-30 09:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:897 on 2025-09-30 09:55_

Yes, unlikely to ever appear in practice. But a nice way to demonstrate how the previous `Type::literal_promotion_type` was different from `Type::promote_literals` (this only works with the latter).

---

_Comment by @github-actions[bot] on 2025-09-30 09:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@sharkdp reviewed on 2025-09-30 09:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:2633 on 2025-09-30 09:56_

Admittedly more of a showcase for the refactor here. I'm also happy to remove this again (and the test above).

---

_Comment by @github-actions[bot] on 2025-09-30 09:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:897 on 2025-09-30 11:52_

> unlikely to ever appear in practice

what do you mean, i write protocols like this all the time

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:2641 on 2025-09-30 12:00_

I wonder if this could/should be something like 

```suggestion
class.known(db).is_none_or(|known_class| !known_class.is_singleton())
```

instead... but it probably doesn't really make a real-world difference. And off-topic for this PR.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1185 on 2025-09-30 12:03_

I'm not actually sure when promotion of module-literal types is desirable -- we might want to remove it from this method. As you discovered on your "remove `| Unknown` from module-globals PR (sorry for looking while it was still in draft 🙈), promotion of module-literal types to `types.ModuleType` can cause serious issues because each module-literal type has a distinct set of attributes available on it. This makes module-literal types somewhat different to all our other literal types

---

_@AlexWaygood approved on 2025-09-30 12:03_

(All my comments are off-topic, but I guess I couldn't resist musing)

---

_@sharkdp reviewed on 2025-09-30 12:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1185 on 2025-09-30 12:17_

Was thinking the exact same thing when I changed this code. In fact, I did arrive here in the first place because promotion of module literals was causing the exact problem you are describing. I'll propose that as a separate change.

Edit: I should have read the rest of your comment before writing mine.

---

_Merged by @sharkdp on 2025-09-30 12:22_

---

_Closed by @sharkdp on 2025-09-30 12:22_

---

_Branch deleted on 2025-09-30 12:22_

---
