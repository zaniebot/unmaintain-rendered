```yaml
number: 12898
title: "[red-knot] fix lookups of possibly-shadowed builtins"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/builtinfix
created_at: 2024-08-15T00:15:48Z
updated_at: 2024-08-15T21:09:31Z
url: https://github.com/astral-sh/ruff/pull/12898
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] fix lookups of possibly-shadowed builtins

---

_Pull request opened by @carljm on 2024-08-15 00:15_

If a builtin is conditionally shadowed by a global, we didn't correctly fall back to builtins for the not-defined-in-globals path (see added test for an example.)

---

_Review requested from @MichaReiser by @carljm on 2024-08-15 00:15_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-15 00:15_

---

_Comment by @github-actions[bot] on 2024-08-15 00:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @carljm on 2024-08-15 02:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1373 on 2024-08-15 06:09_

Nit: 
```suggestion
                            global_symbol_ty_by_name(self.db, self.file, id)
                        };
                        // fallback to builtins
                        if unbound_ty.may_be_unbound(self.db)
                            && Some(self.scope) != builtins_scope(self.db)
                        {
                            Some(unbound_ty.replace_unbound_with(
                                self.db,
                                builtins_symbol_ty_by_name(self.db, id),
                            ))
                        } else {
                            Some(unbound_ty)
                        }
                    } else {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:166 on 2024-08-15 06:10_

Probably a stupid question but: What's the reason that we only do this for unions and not intersections?

---

_@MichaReiser approved on 2024-08-15 06:10_

---

_@carljm reviewed on 2024-08-15 14:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:166 on 2024-08-15 14:31_

It's a great question, and it actually highlights a change I should just make in this PR.

Unbound should never appear in an intersection because if it did, the intersection should simplify to just Unbound. A symbol that must be both Unbound and some other type, is just unbound, the other type constraint is irrelevant.

But I should add a comment to that effect, and also our `IntersectionBuilder` doesn't actually do this yet, so I should add that in this PR as well.

---

_Merged by @carljm on 2024-08-15 21:09_

---

_Closed by @carljm on 2024-08-15 21:09_

---

_Branch deleted on 2024-08-15 21:09_

---
