```yaml
number: 14958
title: "[red-knot] Avoid undeclared path when raising conflicting declarations"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/avoid-undeclared-path
created_at: 2024-12-13T13:42:15Z
updated_at: 2024-12-17T04:19:41Z
url: https://github.com/astral-sh/ruff/pull/14958
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Avoid undeclared path when raising conflicting declarations

---

_Pull request opened by @dhruvmanila on 2024-12-13 13:42_

## Summary

This PR updates the logic when raising conflicting declarations diagnostic to avoid the undeclared path if present.

The conflicting declaration diagnostics is added when there are two or more declarations in the control flow path of a definition whose type isn't equivalent to each other. This can be seen in the following example:

```py
if flag:
	x: int
x = 1  # conflicting-declarations: Unknown, int
```

After this PR, we'd avoid considering "Unknown" as part of the conflicting declarations. This means we'd still flag it for the following case:

```py
if flag:
	x: int
else:
	x: str
x = 1  # conflicting-declarations: int, str
```

A solution that's local to the exception control flow was also explored which required updating the logic for merging the flow snapshot to avoid considering declarations using a flag. This is preserved here: https://github.com/astral-sh/ruff/compare/dhruv/control-flow-no-declarations?expand=1. 

The main motivation to avoid that is we don't really understand what the user experience is w.r.t. the Unknown type and the conflicting-declaration diagnostics. This makes us unsure on what the right semantics are as to whether that diagnostics should be raised or not and when to raise them. For now, we've decided to move forward with this PR and could decide to adopt another solution or remove the conflicting-declaration diagnostics in the future.

Closes: #13966 

## Test Plan

Update the existing mdtest case. Add an additional case specific to exception control flow to verify that the diagnostic is not being raised now.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-13 13:42_

---

_Comment by @github-actions[bot] on 2024-12-13 13:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-12-13 14:55_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-13 14:55_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-13 14:55_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-13 14:55_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-13 14:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:798 on 2024-12-13 18:51_

I think this has an issue: there could be three or more conflicting types, e.g. `int`, `str`, and `Unknown`; this should still emit a diagnostic for the conflict between `int` and `str`.

It's also a little weird that we just search for `Unknown`, because there could be an actual annotation `x: Foo` in one path, where `Foo` resolves to `Unknown` (e.g. because it is imported from a module we can't find), and we would still treat this `Unknown` the same as the undeclared-path `Unknown`.

I think it would be easier to make a more correct fix if we do it inside `declarations_ty` instead, so that in the cases where we don't want to emit the diagnostic, we don't even return an `Err` result at all.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:50 on 2024-12-13 18:51_

Love to see these go away!

---

_@carljm reviewed on 2024-12-13 18:52_

Tests look great! I think we should move the fix inside `declarations_ty`, and add a test for the "three paths, conflicting declarations in two of them" case.

---

_@dhruvmanila reviewed on 2024-12-16 06:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:798 on 2024-12-16 06:37_

Ah yes, thanks for pointing that out. I've pushed a commit that moves the logic to `declarations_ty` which doesn't consider the `undeclared_ty` when looking for conflicting types. It _does_ add the type to the final declared type.

---

_Review requested from @carljm by @dhruvmanila on 2024-12-16 07:10_

---

_@carljm approved on 2024-12-16 20:14_

Looks great, thank you!

---

_Merged by @dhruvmanila on 2024-12-17 04:19_

---

_Closed by @dhruvmanila on 2024-12-17 04:19_

---

_Branch deleted on 2024-12-17 04:19_

---
