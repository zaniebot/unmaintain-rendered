```yaml
number: 20513
title: Update transitive dependencies
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/update-transitive-depndencies
created_at: 2025-09-22T10:01:23Z
updated_at: 2025-09-22T10:50:55Z
url: https://github.com/astral-sh/ruff/pull/20513
synced_at: 2026-01-12T15:57:03Z
```

# Update transitive dependencies

---

_@MichaReiser_

The svg changes are due to https://github.com/rust-cli/anstyle/pull/267 but there's no visible difference. So they're fine to update.

There's a small perf improvement across all benchmarks

---

_Label `internal` added by @MichaReiser on 2025-09-22 10:01_

---

_Comment by @github-actions[bot] on 2025-09-22 10:03_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review requested from @BurntSushi by @MichaReiser on 2025-09-22 10:05_

---

_Comment by @github-actions[bot] on 2025-09-22 10:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-22 10:19_

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

_Comment by @sharkdp on 2025-09-22 10:39_

What? Why is there a mypy_primer change?

---

_Comment by @MichaReiser on 2025-09-22 10:41_

Yeah, I scheduled another mypy primer run because I don't understand this as well. Maybe something something hash map and unstable iteration order?

---

_Comment by @MichaReiser on 2025-09-22 10:43_

The only two dependencies that stand out are Boxcar and Inventory, as they're used by Salsa. 

---

_Comment by @MichaReiser on 2025-09-22 10:50_

Now it's all green...

---

_Merged by @MichaReiser on 2025-09-22 10:50_

---

_Closed by @MichaReiser on 2025-09-22 10:50_

---

_Branch deleted on 2025-09-22 10:50_

---
