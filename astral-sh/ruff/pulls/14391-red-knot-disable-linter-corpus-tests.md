```yaml
number: 14391
title: "[red-knot] Disable linter-corpus tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/disable-corpus-linter-tests
created_at: 2024-11-16T22:22:04Z
updated_at: 2024-11-16T22:35:52Z
url: https://github.com/astral-sh/ruff/pull/14391
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Disable linter-corpus tests

---

_Pull request opened by @sharkdp on 2024-11-16 22:22_

## Summary

Disable the no-panic tests for the linter corpus, as there are too many problems right now, requiring linter-contributors to add their test files to the allow-list.

We can still run the tests using `cargo test -p red_knot_workspace -- --ignored linter_af linter_gz`. This is also why I left the `crates/ruff_linter/` entries in the allow list for now, even if they will get out of sync. But let me know if I should rather remove them.

---

_Label `internal` added by @sharkdp on 2024-11-16 22:22_

---

_Label `red-knot` added by @sharkdp on 2024-11-16 22:22_

---

_Review requested from @carljm by @sharkdp on 2024-11-16 22:22_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-16 22:22_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-16 22:22_

---

_@AlexWaygood approved on 2024-11-16 22:32_

Thank you!

> We can still run the tests using `cargo test -p red_knot_workspace -- --ignored linter_af linter_gz`. This is also why I left the `crates/ruff_linter/` entries in the allow list for now, even if they will get out of sync. But let me know if I should rather remove them.

I think it makes sense to leave the allowlist entries for the linter crate in there. Leaving the entries there probably allows us to examine a specific file in isolation more easily when we're running these tests manually using `--ignored`. That in turn may make it easier to whittle down the files we panic on.

---

_Merged by @sharkdp on 2024-11-16 22:33_

---

_Closed by @sharkdp on 2024-11-16 22:33_

---

_Branch deleted on 2024-11-16 22:33_

---

_Comment by @github-actions[bot] on 2024-11-16 22:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
