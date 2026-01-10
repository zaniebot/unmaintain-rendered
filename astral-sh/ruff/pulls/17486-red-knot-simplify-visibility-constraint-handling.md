```yaml
number: 17486
title: "[red-knot] Simplify visibility constraint handling for `*`-import definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-star-imports
created_at: 2025-04-19T20:45:27Z
updated_at: 2025-04-21T15:33:37Z
url: https://github.com/astral-sh/ruff/pull/17486
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Simplify visibility constraint handling for `*`-import definitions

---

_Pull request opened by @AlexWaygood on 2025-04-19 20:45_

## Summary

A bank-holiday-weekend refactoring PR 😆

I was curious about just how invasive the change would be if I took action on my comment in https://github.com/astral-sh/ruff/pull/17375#issuecomment-2804644637 (optimising the slow path for `*` imports as well as the fast path). It turns out, not very invasive at all! Only one new API has to be added to the use-def map, and it's a fairly innocuous API in my opinion.

I doubt this provides a meaningful speedup in many cases, since it's just very uncommon to use a `*`-import definition to override a pre-existing definition. I'm filing the PR anyway, however, as it ends up simplifying the code in `builder.rs` a fair bit. We no longer have to branch on whether the symbol is newly added or not; we can just do the same thing in both cases.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-04-19 20:45_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-19 20:45_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-19 20:45_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-19 20:45_

---

_Comment by @github-actions[bot] on 2025-04-19 20:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-19 20:55_

neutral on codspeed: https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-star-imports

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:360 on 2025-04-21 13:33_

Doc comment needs to be updated

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:800 on 2025-04-21 13:35_

grammar nit:

```suggestion
    /// - We only apply and negate the visibility constraints to a single symbol, rather than to
```

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:317 on 2025-04-21 13:37_

:exploding_head: We _do_ have `pub(grandparent)` as a possibility, I did not know that! I still worry that we might be over-thinking visibility within this crate, but that is not at all a blocker for this PR.

---

_@dcreager approved on 2025-04-21 13:39_

> I doubt this provides a meaningful speedup in many cases, since it's just very uncommon to use a `*`-import definition to override a pre-existing definition. I'm filing the PR anyway, however, as it ends up simplifying the code in `builder.rs` a fair bit. We no longer have to branch on whether the symbol is newly added or not; we can just do the same thing in both cases.

Simplified code with no performance regression counts as a win to me!

---

_Merged by @AlexWaygood on 2025-04-21 15:33_

---

_Closed by @AlexWaygood on 2025-04-21 15:33_

---

_Branch deleted on 2025-04-21 15:33_

---
