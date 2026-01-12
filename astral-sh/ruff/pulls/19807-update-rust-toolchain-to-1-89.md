```yaml
number: 19807
title: Update Rust toolchain to 1.89
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust189
created_at: 2025-08-07T14:01:05Z
updated_at: 2025-08-07T16:21:53Z
url: https://github.com/astral-sh/ruff/pull/19807
synced_at: 2026-01-12T15:56:47Z
```

# Update Rust toolchain to 1.89

---

_@MichaReiser_

## Summary

This PR updates our Rust toolchain to 1.89. 

It doesn't bump the MSRV. We should do that but I might not have the time today (because it also requires updating conda forge).

The majority of changes are due to the new lifetime lint rule. You can read more here https://blog.rust-lang.org/2025/08/07/Rust-1.89.0/#mismatched-lifetime-syntaxes-lint. Unfortunately, it comes without a fix, but claude was so kind to fix all of them (but I had to prompt it at least twice, as there were more issues and it's lazy to stop)

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-08-07 14:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-07 14:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-07 14:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-07 14:01_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-08-07 14:01_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-08-07 14:01_

---

_Comment by @github-actions[bot] on 2025-08-07 14:03_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-07 14:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` added by @MichaReiser on 2025-08-07 14:05_

---

_@AlexWaygood approved on 2025-08-07 14:06_

Can't say I _love_ the look of all those `<'_>`s, but I guess I'll live with them 😄

---

_@BurntSushi approved on 2025-08-07 14:08_

I like the `<'_>` haha.

---

_Comment by @MichaReiser on 2025-08-07 14:13_

Ah, there are a few more.. 

---

_Comment by @github-actions[bot] on 2025-08-07 16:12_

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

_Merged by @MichaReiser on 2025-08-07 16:21_

---

_Closed by @MichaReiser on 2025-08-07 16:21_

---

_Branch deleted on 2025-08-07 16:21_

---
