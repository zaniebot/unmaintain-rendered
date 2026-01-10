```yaml
number: 19712
title: "[ty] clear the terminal screen in watch mode"
type: pull_request
state: merged
author: leandrobbraga
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: issue-827
created_at: 2025-08-03T14:39:24Z
updated_at: 2025-08-06T22:23:43Z
url: https://github.com/astral-sh/ruff/pull/19712
synced_at: 2026-01-10T17:52:17Z
```

# [ty] clear the terminal screen in watch mode

---

_Pull request opened by @leandrobbraga on 2025-08-03 14:39_

The intended behavior is achieved by entering the terminal alternate screen and clearing the screen before printing the diagnostics.

Fixes [#827](https://github.com/astral-sh/ty/issues/827)


---

_Review requested from @carljm by @leandrobbraga on 2025-08-03 14:39_

---

_Review requested from @MichaReiser by @leandrobbraga on 2025-08-03 14:39_

---

_Review requested from @AlexWaygood by @leandrobbraga on 2025-08-03 14:39_

---

_Review requested from @sharkdp by @leandrobbraga on 2025-08-03 14:39_

---

_Review requested from @dcreager by @leandrobbraga on 2025-08-03 14:39_

---

_Label `ty` added by @AlexWaygood on 2025-08-03 14:41_

---

_Comment by @github-actions[bot] on 2025-08-03 14:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-03 15:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @leandrobbraga on 2025-08-03 17:31_

I've had a look at ruff's codebase. It uses a crate called `clearscreen` and it does not go to an alternate buffer like I've implemented.

What approach do you prefer? Should I be consistent with ruff and use the same crate and make it not go to an alternate buffer?

---

_Label `cli` added by @MichaReiser on 2025-08-04 07:03_

---

_Comment by @MichaReiser on 2025-08-04 07:34_

Sweet, thank you

> I've had a look at ruff's codebase. It uses a crate called `clearscreen` and it does not go to an alternate buffer like I've implemented.
> 
> What approach do you prefer? Should I be consistent with ruff and use the same crate and make it not go to an alternate buffer?

That's a good question. I'm not very familiar with either approach myself. That's why I asked claude for its opinion and it seems that clearscreen is what we want here: https://claude.ai/share/fad8b693-2504-4e9e-b24b-27f010863745

I also don't think that we necessarily have to integrate this into `Printer`. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-04 09:59_

---

_Comment by @leandrobbraga on 2025-08-04 10:03_

Ok, I've changed it to use `clearscreen` instead.

Regarding integrating it into the `Printer` struct, if we aim to be consistent with ruff's codebase, it does integrate with it.

https://github.com/astral-sh/ruff/blob/808c94d509e1e3f3d22218926d08c98381ede7eb/crates/ruff/src/printer.rs#L472-L476

https://github.com/astral-sh/ruff/blob/808c94d509e1e3f3d22218926d08c98381ede7eb/crates/ruff/src/lib.rs#L382-L394

---

_Review comment by @MichaReiser on `crates/ty/src/printer.rs`:115 on 2025-08-04 10:18_

This shouldn't be necessary. The `ty` crate isn't used in wasm
```suggestion
```

---

_@MichaReiser approved on 2025-08-04 10:18_

---

_Comment by @github-actions[bot] on 2025-08-04 10:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2025-08-04 11:45_

---

_Closed by @MichaReiser on 2025-08-04 11:45_

---

_Branch deleted on 2025-08-06 22:23_

---
