```yaml
number: 20951
title: "[ty] Provide completions at the end of the file"
type: pull_request
state: closed
author: sharkdp
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: david/fix-1392
created_at: 2025-10-17T20:34:01Z
updated_at: 2025-10-20T12:55:53Z
url: https://github.com/astral-sh/ruff/pull/20951
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Provide completions at the end of the file

---

_@sharkdp_

## Summary

closes https://github.com/astral-sh/ty/issues/1392

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-10-17 20:34_

---

_Comment by @github-actions[bot] on 2025-10-17 20:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-17 20:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-10-17 21:59_

---

_@MichaReiser reviewed on 2025-10-18 08:00_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:640 on 2025-10-18 08:00_

Nit: Use https://doc.rust-lang.org/std/primitive.slice.html#method.split_last

---

_@sharkdp reviewed on 2025-10-18 18:19_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:640 on 2025-10-18 18:19_

Thanks. This is still in draft because it's authored 100% by Claude and I still wanted to review (and understand) it myself.

---

_Closed by @sharkdp on 2025-10-20 12:55_

---
