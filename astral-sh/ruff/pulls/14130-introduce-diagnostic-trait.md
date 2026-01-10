```yaml
number: 14130
title: "Introduce `Diagnostic` trait"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/structured-diagnostics
created_at: 2024-11-06T11:34:26Z
updated_at: 2024-11-07T12:26:21Z
url: https://github.com/astral-sh/ruff/pull/14130
synced_at: 2026-01-10T20:50:57Z
```

# Introduce `Diagnostic` trait

---

_Pull request opened by @MichaReiser on 2024-11-06 11:34_

## Summary

Extracts the Diagnostic trait from #14116. `check_file` (and `workspace.check`) now returns structured diagnostics instead of plain strings.

I also used this as an opportunity to sort the diagnostics by location.

## Test Plan

`cargo test` and `cargo --bin red_knot`


---

_Review requested from @carljm by @MichaReiser on 2024-11-06 11:34_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-06 11:34_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-06 11:34_

---

_@MichaReiser reviewed on 2024-11-06 11:34_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/edit/range.rs`:1 on 2024-11-06 11:34_

This is copied over from `ruff_server`

---

_Label `red-knot` added by @MichaReiser on 2024-11-06 11:36_

---

_Comment by @github-actions[bot] on 2024-11-06 11:55_

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

_@carljm approved on 2024-11-06 17:32_

Looks good!

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:99 on 2024-11-06 18:01_

This implementation seems unused; do we need it?

---

_@AlexWaygood approved on 2024-11-06 18:02_

---

_@MichaReiser reviewed on 2024-11-06 21:47_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:99 on 2024-11-06 21:47_

We use it in `workspace.check`. 

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:99 on 2024-11-06 21:48_

I deleted it and everything compiled for me?

---

_@AlexWaygood reviewed on 2024-11-06 21:48_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:99 on 2024-11-06 21:54_

Oh right, we use `Box<dyn Diagnostic>` there. It's mainly that this is something that's generally useful. We do use `Arc` and I could also see us using `Box`. 

---

_@MichaReiser reviewed on 2024-11-06 21:54_

---

_@AlexWaygood reviewed on 2024-11-06 21:56_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:99 on 2024-11-06 21:56_

Makes sense.

---

_@dhruvmanila approved on 2024-11-07 11:55_

---

_Merged by @MichaReiser on 2024-11-07 12:26_

---

_Closed by @MichaReiser on 2024-11-07 12:26_

---
