```yaml
number: 21063
title: "[ty] Add `--no-progress` option"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: micha/no-progress
created_at: 2025-10-24T14:07:24Z
updated_at: 2025-10-24T16:00:02Z
url: https://github.com/astral-sh/ruff/pull/21063
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Add `--no-progress` option

---

_@MichaReiser_

## Summary

Add a `--no-progress` option to suppress the progress bar (matching uv's interface)

## Test Plan

I don't think it's possible to test this in our snapshot tests because the progress bar gets hidden for non-interactive terminals.

https://github.com/user-attachments/assets/40468d13-f646-4dc4-85be-04dbdf098248



---

_Label `cli` added by @MichaReiser on 2025-10-24 14:07_

---

_Label `ty` added by @MichaReiser on 2025-10-24 14:07_

---

_Label `cli` added by @MichaReiser on 2025-10-24 14:07_

---

_Label `ty` added by @MichaReiser on 2025-10-24 14:07_

---

_Comment by @github-actions[bot] on 2025-10-24 14:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @MichaReiser on 2025-10-24 14:09_

---

_Review requested from @carljm by @MichaReiser on 2025-10-24 14:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-24 14:09_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-24 14:09_

---

_Comment by @github-actions[bot] on 2025-10-24 14:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-10-24 14:13_

thanks!

---

_@zanieb approved on 2025-10-24 15:57_

---

_@zanieb reviewed on 2025-10-24 15:57_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:15 on 2025-10-24 15:57_

crushed that you changed my API :)

---

_@MichaReiser reviewed on 2025-10-24 15:59_

---

_Review comment by @MichaReiser on `crates/ty/src/printer.rs`:15 on 2025-10-24 15:59_

Sorry. I still very much like the `Printer` API overall. It's nice to have something that's responsible to make these kind of decisions. I hope that makes up for my API change

---

_Merged by @MichaReiser on 2025-10-24 16:00_

---

_Closed by @MichaReiser on 2025-10-24 16:00_

---

_Branch deleted on 2025-10-24 16:00_

---
