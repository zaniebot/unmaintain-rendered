```yaml
number: 15039
title: Upgrade salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/upgrade-salsa
created_at: 2024-12-17T15:39:00Z
updated_at: 2024-12-17T16:04:49Z
url: https://github.com/astral-sh/ruff/pull/15039
synced_at: 2026-01-12T15:55:49Z
```

# Upgrade salsa

---

_@MichaReiser_

The only code change is that Salsa now requires the `Db` to implement `Clone` to create "lightweight" snapshots. 

---

_Review requested from @carljm by @MichaReiser on 2024-12-17 15:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-17 15:39_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-17 15:39_

---

_Label `internal` added by @MichaReiser on 2024-12-17 15:39_

---

_Merged by @MichaReiser on 2024-12-17 15:50_

---

_Closed by @MichaReiser on 2024-12-17 15:50_

---

_Branch deleted on 2024-12-17 15:50_

---

_Comment by @github-actions[bot] on 2024-12-17 15:54_

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

_Comment by @AlexWaygood on 2024-12-17 16:04_

it looks like the fuzz build failed on this PR's CI run... should we make that a required check so that automerge fails if the check fails?

---
