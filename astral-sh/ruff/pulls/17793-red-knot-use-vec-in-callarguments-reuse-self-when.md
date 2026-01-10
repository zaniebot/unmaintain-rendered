```yaml
number: 17793
title: "[red-knot] Use `Vec` in `CallArguments`; reuse `self` when we can"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/vec-arguments
created_at: 2025-05-02T13:39:10Z
updated_at: 2025-05-02T18:37:16Z
url: https://github.com/astral-sh/ruff/pull/17793
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Use `Vec` in `CallArguments`; reuse `self` when we can

---

_Pull request opened by @dcreager on 2025-05-02 13:39_

Quick follow-on to #17788.  If there is no bound `self` parameter, we can reuse the existing `CallArgument{,Type}s`, and we can use a straight `Vec` instead of a `VecDeque`.

---

_Label `red-knot` added by @dcreager on 2025-05-02 13:39_

---

_Review requested from @carljm by @dcreager on 2025-05-02 13:39_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-02 13:39_

---

_Review requested from @sharkdp by @dcreager on 2025-05-02 13:39_

---

_Comment by @github-actions[bot] on 2025-05-02 13:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-05-02 13:46_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:98 on 2025-05-02 13:46_

Could this return `CallArguments` now instead of taking a closure?

---

_@dcreager reviewed on 2025-05-02 13:49_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:98 on 2025-05-02 13:49_

Hmmm...almost. The wrinkle is that I'm trying to avoid cloning if there's no `bound_self`. So I think it could return a `Cow<CallArguments>`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:98 on 2025-05-02 14:01_

Done

---

_@dcreager reviewed on 2025-05-02 14:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:67 on 2025-05-02 14:02_

would it make sense just to derive `Default` for this struct?

---

_@AlexWaygood reviewed on 2025-05-02 14:02_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:67 on 2025-05-02 14:10_

Good call!  I left the `none` constructor since that seems a clearer name at the call site, but it now just delegates to `Self::default`

---

_@dcreager reviewed on 2025-05-02 14:10_

---

_@MichaReiser approved on 2025-05-02 15:25_

---

_Review request for @carljm removed by @carljm on 2025-05-02 15:59_

---

_Merged by @dcreager on 2025-05-02 16:00_

---

_Closed by @dcreager on 2025-05-02 16:00_

---

_Branch deleted on 2025-05-02 16:00_

---

_Comment by @sharkdp on 2025-05-02 18:37_

Thank you!

---
