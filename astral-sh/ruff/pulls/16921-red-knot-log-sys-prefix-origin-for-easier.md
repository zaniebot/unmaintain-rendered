```yaml
number: 16921
title: "[red-knot] Log sys-prefix origin for easier debugging"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/log-sys-prefix-origin
created_at: 2025-03-22T21:40:59Z
updated_at: 2025-03-23T08:07:54Z
url: https://github.com/astral-sh/ruff/pull/16921
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Log sys-prefix origin for easier debugging

---

_@MichaReiser_

## Summary

Log the origin of the sys path prefix. This should help with debugging if someone doesn't understand
why Red Knot picks up a certain venv.

## Test Plan

Ran the CLI and tested that it logs the origin


---

_Review requested from @carljm by @MichaReiser on 2025-03-22 21:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-22 21:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-22 21:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-22 21:41_

---

_Label `red-knot` added by @MichaReiser on 2025-03-22 21:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:227 on 2025-03-22 21:43_

```suggestion
                tracing::debug!("Discovering site-packages paths from sys-prefix `{sys_prefix}` (from {origin})");
```

---

_@AlexWaygood approved on 2025-03-22 21:43_

---

_Comment by @github-actions[bot] on 2025-03-22 21:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-03-23 08:06_

---

_Closed by @MichaReiser on 2025-03-23 08:06_

---

_Branch deleted on 2025-03-23 08:06_

---
