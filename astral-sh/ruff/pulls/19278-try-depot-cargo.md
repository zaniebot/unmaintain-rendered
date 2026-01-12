```yaml
number: 19278
title: Try depot-cargo
type: pull_request
state: closed
author: MichaReiser
labels:
  - ci
assignees: []
draft: true
base: main
head: micha/try-depot-cargo
created_at: 2025-07-11T11:37:56Z
updated_at: 2025-07-16T09:21:50Z
url: https://github.com/astral-sh/ruff/pull/19278
synced_at: 2026-01-12T15:56:35Z
```

# Try depot-cargo

---

_@MichaReiser_

_No description provided._

---

_Label `ci` added by @MichaReiser on 2025-07-11 11:37_

---

_Comment by @github-actions[bot] on 2025-07-14 17:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-14 17:38_

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

_Comment by @MichaReiser on 2025-07-16 09:21_

Using sscache and rust-cache combined is still slower than using rust-cache on its own. Not sure why that is (and this is the advised setup from depot) but that makes it not worth to persue any further.

---

_Closed by @MichaReiser on 2025-07-16 09:21_

---
