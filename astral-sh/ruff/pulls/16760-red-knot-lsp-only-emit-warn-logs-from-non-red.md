```yaml
number: 16760
title: "[red-knot] LSP: only emit WARN logs from non-red-knot sources"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/lsplog
created_at: 2025-03-14T19:22:28Z
updated_at: 2025-03-15T15:47:51Z
url: https://github.com/astral-sh/ruff/pull/16760
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] LSP: only emit WARN logs from non-red-knot sources

---

_Pull request opened by @carljm on 2025-03-14 19:22_

Currently the red-knot LSP server emits any log messages of level `INFO` or higher from non-red-knot crates. This makes its output quite verbose, because Salsa emits an `INFO` level message every time it executes a query. I use red-knot as LSP with neovim, and this spams the log file quite a lot.

It seems like a better default to only emit `WARN` or higher messages from non-red-knot sources.

I confirmed that this fixes the nvim LSP log spam.


---

_Review requested from @MichaReiser by @carljm on 2025-03-14 19:22_

---

_Review requested from @AlexWaygood by @carljm on 2025-03-14 19:22_

---

_Review requested from @sharkdp by @carljm on 2025-03-14 19:22_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-14 19:23_

---

_Comment by @github-actions[bot] on 2025-03-14 19:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review requested from @dhruvmanila by @carljm on 2025-03-14 23:39_

---

_@MichaReiser approved on 2025-03-15 15:21_

We should unify the tracing setup with the CLI but this is an improvement.

---

_Merged by @carljm on 2025-03-15 15:47_

---

_Closed by @carljm on 2025-03-15 15:47_

---

_Branch deleted on 2025-03-15 15:47_

---
