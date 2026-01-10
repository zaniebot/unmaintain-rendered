```yaml
number: 13015
title: "[red-knot] Improve various tracing logs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-module-resolution-logs
created_at: 2024-08-20T16:59:44Z
updated_at: 2024-08-20T18:44:34Z
url: https://github.com/astral-sh/ruff/pull/13015
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Improve various tracing logs

---

_Pull request opened by @AlexWaygood on 2024-08-20 16:59_

Helps with #13011

---

_Review requested from @carljm by @AlexWaygood on 2024-08-20 16:59_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-20 16:59_

---

_Label `red-knot` added by @AlexWaygood on 2024-08-20 17:00_

---

_Renamed from "[red-knot] Improve some tracing logs" to "[red-knot] Improve some module-resolution tracing logs" by @AlexWaygood on 2024-08-20 17:00_

---

_Comment by @github-actions[bot] on 2024-08-20 17:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[red-knot] Improve some module-resolution tracing logs" to "[red-knot] Improve various tracing logs" by @AlexWaygood on 2024-08-20 17:43_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:928 on 2024-08-20 18:11_

I would make this a standalone function. Makes it easier to call.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:928 on 2024-08-20 18:12_

I'm not if we want to both log attempting and the resolved module. It seems relatively verbose.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:965 on 2024-08-20 18:13_

Same here. We should avoid making debug too verbose. Maybe consider making it a trace or removing it entirely 

---

_@MichaReiser approved on 2024-08-20 18:15_

Thanks 

---

_@AlexWaygood reviewed on 2024-08-20 18:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:965 on 2024-08-20 18:33_

I made it a trace

---

_Merged by @AlexWaygood on 2024-08-20 18:34_

---

_Closed by @AlexWaygood on 2024-08-20 18:34_

---

_Branch deleted on 2024-08-20 18:34_

---
