```yaml
number: 20046
title: "[ty] Disallow std::env and io methods in most ty crates"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/disallow-std-io-env-methods
created_at: 2025-08-22T15:33:03Z
updated_at: 2025-08-22T18:13:49Z
url: https://github.com/astral-sh/ruff/pull/20046
synced_at: 2026-01-12T15:56:53Z
```

# [ty] Disallow std::env and io methods in most ty crates

---

_@MichaReiser_

## Summary

We use the `System` abstraction in ty to abstract away the host/system on which ty runs. 
This has a few benefits:

* Tests can run in full isolation using a memory system (that uses an in-memory file system)
* The LSP has a custom implementation where `read_to_string` returns the content as seen by the editor (e.g. unsaved changes) instead of always returning the content as it is stored on disk
* We don't require any file system polyfills for wasm in the browser


However, it does require extra care that we don't accidentally use `std::fs` or `std::env` (etc.) methods in ty's code base (which is very easy).

This PR sets up Clippy and disallows the most common methods, instead pointing users towards the corresponding `System` methods. 

The setup is a bit awkward because clippy doesn't support inheriting configurations. That means, a crate can only override the entire workspace configuration or not at all. 
The approach taken in this PR is:

* Configure the disallowed methods at the workspace level
* Allow `disallowed_methods` at the workspace level
* Enable the lint at the crate level using the warn attribute (in code)


The obvious downside is that it won't work if we ever want to disallow other methods, but we can figure that out once we reach that point.

What about false positives: Just add an `allow` and move on with your life :) This isn't something that we have to enforce strictly; the goal is to catch accidental misuse.

## Test Plan

Clippy found a place where we incorrectly used `std::fs::read_to_string`


---

_Label `internal` added by @MichaReiser on 2025-08-22 15:33_

---

_Label `ty` added by @MichaReiser on 2025-08-22 15:33_

---

_Review requested from @carljm by @MichaReiser on 2025-08-22 15:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-22 15:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-22 15:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-22 15:33_

---

_Comment by @github-actions[bot] on 2025-08-22 15:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-22 15:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-08-22 15:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:846 on 2025-08-22 15:39_

This is a bit annoying. I don't love it but the only alternative that I can think of is to a) use a thread local or b) have a global `System` somewhere (which breaks test isolation)

---

_Comment by @MichaReiser on 2025-08-22 15:40_

Feel free to hit merge (or not merge ;))

---

_Comment by @github-actions[bot] on 2025-08-22 15:49_

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

_@AlexWaygood reviewed on 2025-08-22 17:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:846 on 2025-08-22 17:57_

Huh, yeah, I was initially surprised at these changes but I also don't see a better solution

---

_@AlexWaygood approved on 2025-08-22 17:57_

---

_@carljm approved on 2025-08-22 18:13_

---

_Merged by @carljm on 2025-08-22 18:13_

---

_Closed by @carljm on 2025-08-22 18:13_

---

_Branch deleted on 2025-08-22 18:13_

---
