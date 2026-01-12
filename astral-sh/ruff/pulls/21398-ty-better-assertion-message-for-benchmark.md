```yaml
number: 21398
title: "[ty] Better assertion message for benchmark diagnostics check"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/better-panic-msg
created_at: 2025-11-12T09:43:41Z
updated_at: 2025-11-12T10:18:54Z
url: https://github.com/astral-sh/ruff/pull/21398
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Better assertion message for benchmark diagnostics check

---

_@sharkdp_

I don't know why, but it always takes me an eternity to find the failing project name a few lines below in the output. So I'm suggesting we just add the project name to the assertion message.

---

_Label `internal` added by @sharkdp on 2025-11-12 09:43_

---

_Label `ty` added by @sharkdp on 2025-11-12 09:43_

---

_Review requested from @MichaReiser by @sharkdp on 2025-11-12 09:43_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty_walltime.rs`:74 on 2025-11-12 09:44_

Was thinking about using `db.project().name(db)` instead of passing in the `project_name`...

---

_@sharkdp reviewed on 2025-11-12 09:44_

---

_@AlexWaygood approved on 2025-11-12 09:50_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 09:55_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Merged by @sharkdp on 2025-11-12 10:02_

---

_Closed by @sharkdp on 2025-11-12 10:02_

---

_Branch deleted on 2025-11-12 10:02_

---

_@MichaReiser reviewed on 2025-11-12 10:07_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty_walltime.rs`:74 on 2025-11-12 10:07_

but? (not that I mind much either way)

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty_walltime.rs`:74 on 2025-11-12 10:18_

My guess was that this would have taken the actual project name from the `pyproject.toml`, which might be different from "our" name for the benchmark?

---

_@sharkdp reviewed on 2025-11-12 10:18_

---
