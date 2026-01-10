```yaml
number: 20726
title: Add missing import to example doc
type: pull_request
state: closed
author: ShaharNaveh
labels: []
assignees: []
base: main
head: docs-missing-import
created_at: 2025-10-06T19:15:58Z
updated_at: 2025-10-07T16:08:58Z
url: https://github.com/astral-sh/ruff/pull/20726
synced_at: 2026-01-10T17:34:34Z
```

# Add missing import to example doc

---

_Pull request opened by @ShaharNaveh on 2025-10-06 19:15_

Feel free to close this PR if you think it's noise

---

_Review requested from @carljm by @ShaharNaveh on 2025-10-06 19:15_

---

_Review requested from @AlexWaygood by @ShaharNaveh on 2025-10-06 19:15_

---

_Review requested from @sharkdp by @ShaharNaveh on 2025-10-06 19:15_

---

_Review requested from @dcreager by @ShaharNaveh on 2025-10-06 19:15_

---

_Comment by @github-actions[bot] on 2025-10-06 19:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-06 19:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @carljm on 2025-10-07 16:08_

Thanks for the PR, but I don't think this helps the example communicate better. It's a snippet, not necessarily a complete module, and I think the intent of `secrets.randbelow(2)` is already sufficiently clear.

---

_Closed by @carljm on 2025-10-07 16:08_

---
