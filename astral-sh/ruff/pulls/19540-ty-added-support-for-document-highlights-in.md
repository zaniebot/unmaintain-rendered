```yaml
number: 19540
title: "[ty] Added support for document highlights in playground."
type: pull_request
state: merged
author: UnboundVariable
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: highlights_playground
created_at: 2025-07-24T20:29:36Z
updated_at: 2025-07-25T15:55:44Z
url: https://github.com/astral-sh/ruff/pull/19540
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Added support for document highlights in playground.

---

_@UnboundVariable_

This PR adds support for the "document highlights" feature in the ty playground.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-24 20:29_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-24 20:29_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-24 20:29_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-24 20:29_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-24 20:29_

---

_@UnboundVariable reviewed on 2025-07-24 20:31_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/goto.rs`:311 on 2025-07-24 20:31_

Note: I purposely removed this TODO. It's unrelated to the other parts of this PR, but this was a convenient time to remove it. I previously thought that "goto" for TypedDict subscripts was supported in pylance, but it turns out it is not supported. While this is something that could certainly be added at some point in the future, we should probably wait for users to request it before making it a priority.

---

_Comment by @github-actions[bot] on 2025-07-24 20:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-24 21:22_

---

_Label `playground` added by @MichaReiser on 2025-07-25 06:36_

---

_Label `ty` added by @MichaReiser on 2025-07-25 06:36_

---

_@MichaReiser approved on 2025-07-25 06:37_

Thank you. I'll waiting for the moment where this all becomes too much and we'll need to rewrite the playground to run ty in a web worker. Probably something that claude can help us when we reach that point

---

_Merged by @UnboundVariable on 2025-07-25 15:55_

---

_Closed by @UnboundVariable on 2025-07-25 15:55_

---

_Branch deleted on 2025-07-25 15:55_

---
