```yaml
number: 15685
title: Tidy knot CLI tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/tidy-cli-tests
created_at: 2025-01-23T09:47:28Z
updated_at: 2025-01-23T14:10:50Z
url: https://github.com/astral-sh/ruff/pull/15685
synced_at: 2026-01-10T20:05:43Z
```

# Tidy knot CLI tests

---

_Pull request opened by @MichaReiser on 2025-01-23 09:47_

## Summary
Introduce some helpers to remvoe the noise around writing files.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-01-23 09:47_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-23 09:47_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-23 09:47_

---

_Label `testing` added by @MichaReiser on 2025-01-23 09:47_

---

_Label `red-knot` added by @MichaReiser on 2025-01-23 09:47_

---

_Comment by @github-actions[bot] on 2025-01-23 09:58_

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

_@sharkdp reviewed on 2025-01-23 12:40_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:298 on 2025-01-23 12:40_

What would it mean for a test to write to an absolute path? Wouldn't that escape the temporary directory and potentially pollute/corrupt the developer's filesystem?

---

_@sharkdp reviewed on 2025-01-23 12:42_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:302 on 2025-01-23 12:42_

Why do we prepend `self.project_dir` again here?

---

_@MichaReiser reviewed on 2025-01-23 12:49_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:298 on 2025-01-23 12:49_

I think it's fair to say that we should not support absolute paths for now. 

---

_@sharkdp approved on 2025-01-23 13:02_

That looks much nicer — thanks!

---

_Merged by @MichaReiser on 2025-01-23 13:06_

---

_Closed by @MichaReiser on 2025-01-23 13:06_

---

_Branch deleted on 2025-01-23 13:06_

---
