```yaml
number: 13400
title: "[red-knot] inferred type, not Unknown, for undeclared paths"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/declared-vs-non
created_at: 2024-09-18T22:44:47Z
updated_at: 2024-09-19T17:54:40Z
url: https://github.com/astral-sh/ruff/pull/13400
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] inferred type, not Unknown, for undeclared paths

---

_Pull request opened by @carljm on 2024-09-18 22:44_

After looking at more cases (for example, the case in the added test in this PR), I realized that our previous rule, "if a symbol has any declarations, use only declarations for its public type" is not adequate. Rather than using `Unknown` as fallback if the symbol is not declared in some paths, we need to use the inferred type as fallback in that case.

For the paths where the symbol _was_ declared, we know that any bindings must be assignable to the declared type in that path, so this won't change the overall declared type in those paths. But for paths where the symbol wasn't declared, this will give us a better type in place of `Unknown`.

---

_Review requested from @MichaReiser by @carljm on 2024-09-18 22:44_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-18 22:44_

---

_Comment by @github-actions[bot] on 2024-09-18 22:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @carljm on 2024-09-18 23:36_

---

_@AlexWaygood approved on 2024-09-19 02:33_

---

_Merged by @carljm on 2024-09-19 04:47_

---

_Closed by @carljm on 2024-09-19 04:47_

---

_Branch deleted on 2024-09-19 04:47_

---

_Comment by @MichaReiser on 2024-09-19 06:00_

> After looking at more cases (for example, the case in the added test in this PR), I realized that our previous rule, "if a symbol has any declarations, use only declarations for its public type" is not adequate. Rather than using Unknown as fallback if the symbol is not declared in some paths, we need to use the inferred type as fallback in that case.

Could you share some of those examples? It would be helpful if we look back to this PR in a few weeks and wonder why we changed it the way we did

---

_@MichaReiser reviewed on 2024-09-19 06:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:202 on 2024-09-19 06:02_

Is it now possible that we emit one extra error if the `undeclared_ty` is not compatible with any of the declared ty?

---

_Comment by @carljm on 2024-09-19 17:47_

> Could you share some of those examples? It would be helpful if we look back to this PR in a few weeks and wonder why we changed it the way we did

I think the test I added summarizes the essential character of all the cases I considered; it's just the scenario where a name is only conditionally declared in a module, but is always bound, and then you import that name from another module.

(I wasn't looking at cases like this in real code, so I don't have those to point to, though I'm sure they exist. These were just additional cases I was thinking through after running into the real case in typeshed where a name is imported in one path and declared in another.)

---

_@carljm reviewed on 2024-09-19 17:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:202 on 2024-09-19 17:51_

That was already (intentionally) the case before, it's just that before `undeclared_ty` was hardcoded to `Unknown`. And `Unknown` is not equivalent to any other type, so it was already previously guaranteed to be an error if you tried assigning to a name that had been declared in one path but not in another.

Also, there are two cases where we check the declared type. One is for the public type (e.g. importing a symbol); this use case doesn't care if there are conflicts (that's not a problem the importing module cares about, it will just take the union and go with it). And this use case is the only one that now sets `undeclared_ty` to the inferred type.

The other use case is in `add_binding` to decide if an assignment is allowed. That's the only one that emits a diagnostic if there are conflicts. And that use case didn't change at all in this PR, it still always sets `undeclared_ty` to `Unknown` (the only difference is that that moved out of this function and into `add_binding`.)

So there's no change in this PR to what diagnostics will be emitted due to conflicting declared types.

---
