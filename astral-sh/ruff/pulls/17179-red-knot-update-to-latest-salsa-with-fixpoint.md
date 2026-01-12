```yaml
number: 17179
title: "[red-knot] update to latest Salsa with fixpoint caching fix"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/bumpsalsa2
created_at: 2025-04-03T14:43:11Z
updated_at: 2025-04-03T16:07:40Z
url: https://github.com/astral-sh/ruff/pull/17179
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] update to latest Salsa with fixpoint caching fix

---

_@carljm_

With this PR, we no longer "hang" (not actually a hang, just an explosion in execution time) when checking pylint.

---

_Label `red-knot` added by @AlexWaygood on 2025-04-03 14:50_

---

_Comment by @github-actions[bot] on 2025-04-03 14:52_

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

_Renamed from "[WIP] try Salsa with fixpoint fix" to "[red-knot] update to latest Salsa with fixpoint caching fix" by @carljm on 2025-04-03 15:58_

---

_Marked ready for review by @carljm on 2025-04-03 15:58_

---

_Review requested from @MichaReiser by @carljm on 2025-04-03 16:00_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-03 16:00_

---

_@MichaReiser approved on 2025-04-03 16:02_

---

_Comment by @MichaReiser on 2025-04-03 16:03_

How long before @sharkdp adds new projects to our mypy primer run? :)

---

_Comment by @AlexWaygood on 2025-04-03 16:04_

I shall put in my bet at "7 minutes"

---

_Comment by @carljm on 2025-04-03 16:04_

> How long before @sharkdp adds new projects to our mypy primer run? :)

I think the projects that no longer hang still panic at the moment, that's next on my list...

---

_Merged by @carljm on 2025-04-03 16:05_

---

_Closed by @carljm on 2025-04-03 16:05_

---

_Branch deleted on 2025-04-03 16:05_

---

_Comment by @AlexWaygood on 2025-04-03 16:06_

Ah yes, I see we still panic on pylint with this fix landed (twice, in fact!), but we now promptly finish the process after panicking, whereas previously we panicked and then... hung around for ages before finishing the process 😄

Progress!

---
