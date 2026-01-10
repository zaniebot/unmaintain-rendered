```yaml
number: 17375
title: "[red-knot] Further optimize `*`-import visibility constraints"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-star-imports
created_at: 2025-04-13T17:49:05Z
updated_at: 2025-04-15T11:32:24Z
url: https://github.com/astral-sh/ruff/pull/17375
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Further optimize `*`-import visibility constraints

---

_Pull request opened by @AlexWaygood on 2025-04-13 17:49_

## Summary

Fixes #17322. Remove any need for snapshotting when applying visibility constraints to `*` imports. This also ends up simplifying the code!

## Test Plan

Existing tests all pass. I'd appreciate a careful review, though. Is it definitely safe to remove these calls, as I am doing currently in the PR?

E.g. the `UseDefMapBuilder::negate_star_import_visibility_constraint` method on `main` has this call:

```rs
        self.scope_start_visibility = self
            .visibility_constraints
            .add_and_constraint(self.scope_start_visibility, negated_constraint);
```

and `UseDefMapBuilder::merge` has this call:

```rs
        self.scope_start_visibility = self
            .visibility_constraints
            .add_or_constraint(self.scope_start_visibility, snapshot.scope_start_visibility);
```

Both calls are currently removed in this PR for the "fast path" for newly added `*`-import definitions, since `UseDefMapBuilder::negate_star_import_visibility_constraint` is removed as part of this PR, and `UseDefMapBuilder::merge` is no longer called in the fast path. If it's _not_ safe to remove these calls from the fast path, is there a test I could add that passes on `main`, but fails with them removed?


---

_Label `red-knot` added by @AlexWaygood on 2025-04-13 17:49_

---

_Comment by @github-actions[bot] on 2025-04-13 17:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-13 17:58_

3% speedup on the cold benchmark 🥳 https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-star-imports

---

_Label `performance` added by @AlexWaygood on 2025-04-13 17:58_

---

_Marked ready for review by @AlexWaygood on 2025-04-13 18:04_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-13 18:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-13 18:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-13 18:04_

---

_@sharkdp approved on 2025-04-14 08:45_

Thank you, this looks correct! Great that we can reclaim even more performance.

`self.scope_start_visibility` tracks the visibility of `x_new = <unbound>` bindings of symbols that are *not yet defined* (symbols that *are* already defined, track visibility of their own `x_defined = <unbound>` binding). This is important when we model something like
```py
if True:
    x = 1
else:
    # At this point in control flow, we have a scope_start_visibility
    # of `~[True]`, which we'll use for the `x = <unbound>` binding
    # that will be implicitly created for this control flow branch
    # when we merge control flow with the `if` branch below.

# a use of `x` here sees `x = 1` with visibility `[True]` and
# `x = <unbound>` with visibility `~[True]`.
```

The important thing is that `scope_start_visibility` returns to `AlwaysTrue` after this `if`-`else` branch. `x` can not be unbound, but *new* symbols would still be unbound. You can basically ask: if I would see a use of `has_not_been_bound_yet` at this point in control flow, would I still be able to see the implicit `has_not_been_bound_yet = <unbound>` binding at the scope start? And the answer for this example is yes. Things could be different if there would be terminal statements inside the branches:
```py
def _():
    if True:
        return

    # scope_start_visibility would be AlwaysFalse here
```

For the star-import construct that we're modeling, the scope-start-visibility should not be affected (after control flow has merged), since there are no terminal statements inside. A use of `has_not_been_bound_yet` after this construct would have the same visibility of the scope start than before:
```py
if <condition>:
    from module import X
else:
    # empty
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:753 on 2025-04-14 22:45_

I think it would be useful to future-us to add a doc comment to this method explaining what it is doing and why it is safe to do it, in the limited cases where we use this function.

---

_@carljm approved on 2025-04-14 22:45_

Very nice, thank you!!

---

_Comment by @AlexWaygood on 2025-04-15 10:51_

I think there are probably some optimisations we could also make for the "slow path" where the `*` import redefines a symbol that already exists in the global namespace. Even in the slow path, we're probably doing much more work than we need to by snapshotting all definition/declaration states -- we really only need to snapshot the definition/declaration states for a single symbol.

But I don't know if it's worth the complexity cost to make that optimisation, since I think it would require adding more APIs to the use-def map that would only be used in this one place, and we probably don't hit the slow path very often at all.

---

_@AlexWaygood reviewed on 2025-04-15 11:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:753 on 2025-04-15 11:20_

Added some docs! Sadly this PR is no longer net-negative in terms of whether it adds or removes lines of code, but so it goes 🥲

Would appreciate a quick review from you or @sharkdp to double-check everything in the doc-comment is accurate!

---

_@sharkdp approved on 2025-04-15 11:31_

The new comment looks great, thank you

---

_Merged by @AlexWaygood on 2025-04-15 11:32_

---

_Closed by @AlexWaygood on 2025-04-15 11:32_

---

_Branch deleted on 2025-04-15 11:32_

---
