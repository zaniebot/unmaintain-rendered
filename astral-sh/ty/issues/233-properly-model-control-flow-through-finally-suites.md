```yaml
number: 233
title: "Properly model control flow through `finally` suites"
type: issue
state: open
author: AlexWaygood
labels:
  - control flow
  - runtime semantics
assignees: []
created_at: 2024-10-31T12:09:49Z
updated_at: 2025-11-18T16:10:24Z
url: https://github.com/astral-sh/ty/issues/233
synced_at: 2026-01-10T01:58:59Z
```

# Properly model control flow through `finally` suites

---

_Issue opened by @AlexWaygood on 2024-10-31 12:09_

`finally` suites in `try`/`except` blocks have very complex control flow, which we don't yet model, and need to.

I've already written extensively about the complexities of `finally` blocks, so I won't try to summarise them again here. Instead, I'll point to:
- This TODO comment here: https://github.com/astral-sh/ruff/blob/76e4277696e8b34b771d554f8f0d07054d413289/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L929-L939
- The various `finally`-related TODO comments in our exception-handler control-flow test suite, from this point onwards: https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md#exception-handlers-with-finally-branches-but-no-except-branches
- The full writeup of exception-handler control flow I put together here: https://astral-sh.notion.site/Exception-handler-control-flow-11348797e1ca80bb8ce1e9aedbbe439d


---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-10-31 12:09_

---

_Renamed from "[red-knot] Properly model control flow through `finally` suites" to "Properly model control flow through `finally` suites" by @MichaReiser on 2025-05-07 15:27_

---

_Label `control flow` added by @AlexWaygood on 2025-05-10 21:38_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:48_

---

_Unassigned @AlexWaygood by @MichaReiser on 2025-08-15 12:21_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 15:47_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
