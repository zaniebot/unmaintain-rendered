```yaml
number: 17855
title: Show a warning at the end of the diagnostic list if there are any fatal warnings
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/warn-panic
created_at: 2025-05-05T12:55:50Z
updated_at: 2025-05-06T07:33:09Z
url: https://github.com/astral-sh/ruff/pull/17855
synced_at: 2026-01-12T15:56:06Z
```

# Show a warning at the end of the diagnostic list if there are any fatal warnings

---

_@MichaReiser_

## Test Plan

```
error: panic: Panicked at crates/ty_python_semantic/src/types/infer.rs:7321:9 when checking `test.py`: `Test`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["..."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(166f))
             at crates/ty_python_semantic/src/types/infer.rs:183
   1: signature_(Id(5413))
             at crates/ty_python_semantic/src/types.rs:6559
   2: infer_scope_types(Id(1004))
             at crates/ty_python_semantic/src/types/infer.rs:118
   3: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:83


Found 1 diagnostic
WARN A panic occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.


```

---

_Label `ty` added by @MichaReiser on 2025-05-05 12:55_

---

_Marked ready for review by @MichaReiser on 2025-05-05 12:56_

---

_Review requested from @carljm by @MichaReiser on 2025-05-05 12:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-05 12:56_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-05 12:56_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-05 12:56_

---

_Comment by @github-actions[bot] on 2025-05-05 12:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-05-05 13:04_

---

_Review comment by @AlexWaygood on `crates/ty/src/main.rs`:310 on 2025-05-05 13:04_

Could we mention that the panic will be the diagnostic with the `[fatal]` severity? It might make it easier for users to figure out which one in the diagnostic list was the panic

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:310 on 2025-05-05 13:19_

I can try

---

_@MichaReiser reviewed on 2025-05-05 13:19_

---

_@carljm reviewed on 2025-05-06 00:08_

---

_Review comment by @carljm on `crates/ty/src/main.rs`:310 on 2025-05-06 00:08_

I would just say "A fatal error occurred" instead of "A panic occurred" -- I think that makes it clear enough that you're looking for the error with `[fatal]` severity. And "panic" is kind of Rust-specific terminology.

---

_@carljm approved on 2025-05-06 00:09_

---

_@sharkdp approved on 2025-05-06 06:19_

Thank you

---

_Merged by @MichaReiser on 2025-05-06 07:14_

---

_Closed by @MichaReiser on 2025-05-06 07:14_

---

_Branch deleted on 2025-05-06 07:14_

---

_Review comment by @AlexWaygood on `crates/ty/src/main.rs`:310 on 2025-05-06 07:33_

I think this should be "a fatal error occurred" rather than "a fatal occurred"

---

_@AlexWaygood reviewed on 2025-05-06 07:33_

---
