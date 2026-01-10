```yaml
number: 16503
title: "[red-knot] Add new `Diagnostic` data type"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/new-diag-type
created_at: 2025-03-04T17:50:25Z
updated_at: 2025-03-05T13:23:04Z
url: https://github.com/astral-sh/ruff/pull/16503
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add new `Diagnostic` data type

---

_Pull request opened by @BurntSushi on 2025-03-04 17:50_

This also moves most (but not all) of the existing diagnostic
infrastructure into an `old` submodule, and renames the types to
include "old" in the name. The intent is for the `old` submodule to be
deleted.

Otherwise, the new `Diagnostic` data type is meant to capture
our diagnostic data model (discussed internally). I expect the
implementation to be tweaked over time (there isn't too much
interesting going on yet), but I think this is a good base to start
with.

Note that this doesn't include fixes or diffs. I think we're punting
on doing too much with that for right now.

Closes #16505


---

_Review requested from @carljm by @BurntSushi on 2025-03-04 17:50_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-03-04 17:50_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-03-04 17:50_

---

_Review requested from @sharkdp by @BurntSushi on 2025-03-04 17:50_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-04 17:57_

---

_Label `diagnostics` added by @BurntSushi on 2025-03-04 18:08_

---

_Comment by @github-actions[bot] on 2025-03-04 18:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/old.rs`:349 on 2025-03-04 18:34_

I don't think we' have to rename this diagnostic, similar to how you didn't rename `TypeCheckDiagnostic`. This is a concrete diagnostic that will implement `IntoDiagnostic` under your new model. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:32 on 2025-03-04 18:36_

We could use the [`drop_bomb`](https://docs.rs/drop_bomb/latest/drop_bomb/) type for this, instead of wrapping it in an `Option`. It has the advantage that it doesn't require the more awkward `Option` access pattern. You can also choose a `DebugBomb` that is disabled in release (red knot panicking because of a dropped diagnostic in production doesn't seem the right trade off to me)

The additional `Box`ing might be unfortunate for Red Knot where we have to also `Arc` them to make them cheap clonable. Although that depends if Red Knot will propagate `TypeCheckDiagnostic`s or `Diagnostic`s. It's also just one additional allocation that shouldn't matter much. Let's ignore this for now.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:311 on 2025-03-04 19:42_

It's somewhat unfortunate that git thinks everything below here is new when it isn't. It would be nice to keep the history 

---

_@MichaReiser approved on 2025-03-04 19:44_

This is great. A few smaller nit comments, nothing major

---

_@carljm approved on 2025-03-04 20:13_

Looks great to me!! Looking forward to being able to use this to improve our existing diagnostics.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/old.rs`:349 on 2025-03-05 12:42_

For `TypeCheckDiagnostic`, I think that was mostly just an oversight because it wasn't defined in `ruff_db`. I expect `TypeCheckDiagnostic`, as it currently exists, to likely be deleted.

`ParseDiagnostic` (or `OldParseDiagnostic`) might still hang around. I'm actually not sure. It could definitely implement `IntoDiagnostic` (when that trait is defined).

But this type exists to integrate with the old diagnostic trait, so I think I'd prefer to just mark it as "old" for now.

---

_@BurntSushi reviewed on 2025-03-05 12:42_

---

_@BurntSushi reviewed on 2025-03-05 13:06_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:32 on 2025-03-05 13:06_

That's not the kind of crate I would normally use to be honest. It's tiny! But it looks like we're already using it, so I might as well. Although, if you want to give a good error message (e.g., with the diagnostic ID or something), then it forces you to allocate a string up-front (even if it's a fake bomb, as it would be in release mode) instead of just creating the message in the `Drop` impl itself. But maybe that's fine.

The point about `debug_assertions` is a good one.

I'll just switch to `drop_bomb` for now. ... Except it doesn't implement `Clone`.

This is silly. I just need a simple `bool` gated on `cfg(debug_assertions)`. And then we can have a nicer panic message. So... concerns addressed I think, without using `drop_bomb`. :-)

---

_@BurntSushi reviewed on 2025-03-05 13:07_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:311 on 2025-03-05 13:07_

:-(

Sometimes using `--follow` can help when browsing git history. Although it is hit-or-miss in my experience.

---

_@BurntSushi reviewed on 2025-03-05 13:09_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:32 on 2025-03-05 13:09_

> The additional Boxing might be unfortunate for Red Knot where we have to also Arc them to make them cheap clonable. Although that depends if Red Knot will propagate TypeCheckDiagnostics or Diagnostics. It's also just one additional allocation that shouldn't matter much. Let's ignore this for now.

I wasn't sure what the needed pattern was, so I used `Box`.

If we really do want the `Arc`, then I can make `Diagnostic` use an `Arc` with `Arc::make_mut` that will only copy if the ref count is greater than `1`. Then callers can freely and cheaply clone, and only pay for it if you need mutation after cloning. But yeah for now, I'll just leave it as a `Box`.

---

_Merged by @BurntSushi on 2025-03-05 13:23_

---

_Closed by @BurntSushi on 2025-03-05 13:23_

---

_Branch deleted on 2025-03-05 13:23_

---
