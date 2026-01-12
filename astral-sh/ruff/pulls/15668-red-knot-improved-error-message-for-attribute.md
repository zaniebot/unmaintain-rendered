```yaml
number: 15668
title: "[red-knot] Improved error message for attribute-assignments"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/attribute-assignment-improved-error
created_at: 2025-01-22T10:25:49Z
updated_at: 2025-01-22T11:06:00Z
url: https://github.com/astral-sh/ruff/pull/15668
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Improved error message for attribute-assignments

---

_@sharkdp_

## Summary

Slightly improved error message for attribute assignments as per [this suggestion](https://github.com/astral-sh/ruff/pull/15613#discussion_r1923601952).

## Test Plan

—

---

_Label `red-knot` added by @sharkdp on 2025-01-22 10:25_

---

_Review requested from @carljm by @sharkdp on 2025-01-22 10:25_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-22 10:25_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-22 10:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:105 on 2025-01-22 10:26_

@AlexWaygood I intentionally changed "public type" to just "type" here, as I'm not sure if "public type" is a term that we want to teach to users?

---

_@sharkdp reviewed on 2025-01-22 10:26_

---

_@sharkdp reviewed on 2025-01-22 10:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:992 on 2025-01-22 10:27_

Let me know if you prefer another function for this to avoid the `Option<&'static str>` parameter. I attempted to get better error messages in even more cases (see other comment), but haven't really found any so far.

---

_@AlexWaygood reviewed on 2025-01-22 10:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:105 on 2025-01-22 10:28_

yeah, you're probably right -- it's probably unnecessary jargon

---

_@sharkdp reviewed on 2025-01-22 10:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:881 on 2025-01-22 10:30_

I tried for a while to extract anything useful from `binding.kind(db)`, i.e. the `DefinitionKind` enum. But it didn't change any existing diagnostic messages. And all instances of "Object of type … is not assignable to …" messages I went through would be cases of "… is not assignable to *variable of type* …", which I'm not sure adds a lot of value.

---

_Comment by @github-actions[bot] on 2025-01-22 10:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:105 on 2025-01-22 10:32_

I suppose even better might be if we included the name of the attribute?

```suggestion
# error: [invalid-assignment] "Object of type `Literal[1]` is not assignable to attribute `pure_instance_variable` of type `str`"
```

---

_@AlexWaygood approved on 2025-01-22 10:32_

Thank you!

---

_Merged by @sharkdp on 2025-01-22 11:04_

---

_Closed by @sharkdp on 2025-01-22 11:04_

---

_Branch deleted on 2025-01-22 11:04_

---
