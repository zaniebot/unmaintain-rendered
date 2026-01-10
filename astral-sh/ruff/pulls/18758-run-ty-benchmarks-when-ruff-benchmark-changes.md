```yaml
number: 18758
title: "Run ty benchmarks when `ruff_benchmark` changes"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/ty-run-on-benchmark-changes
created_at: 2025-06-18T15:04:06Z
updated_at: 2025-06-18T15:43:20Z
url: https://github.com/astral-sh/ruff/pull/18758
synced_at: 2026-01-10T18:39:08Z
```

# Run ty benchmarks when `ruff_benchmark` changes

---

_Pull request opened by @MichaReiser on 2025-06-18 15:04_

I noticed that the walltime benchmarks didn't run in https://github.com/astral-sh/ruff/pull/18757

---

_Label `testing` added by @MichaReiser on 2025-06-18 15:04_

---

_Label `ty` added by @MichaReiser on 2025-06-18 15:04_

---

_@AlexWaygood approved on 2025-06-18 15:07_

hah, you beat me to it. Though the version I was working on locally adds a separate flag that indicated if any code inside `ruff_benchmark` changed. But this also seems fine.

---

_Comment by @github-actions[bot] on 2025-06-18 15:16_

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

_Merged by @MichaReiser on 2025-06-18 15:43_

---

_Closed by @MichaReiser on 2025-06-18 15:43_

---

_Branch deleted on 2025-06-18 15:43_

---
