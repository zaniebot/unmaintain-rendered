```yaml
number: 15694
title: Preserve raw string prefix and escapes
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/raw-string
created_at: 2025-01-23T15:40:25Z
updated_at: 2025-01-23T17:12:13Z
url: https://github.com/astral-sh/ruff/pull/15694
synced_at: 2026-01-10T19:57:22Z
```

# Preserve raw string prefix and escapes

---

_Pull request opened by @ntBre on 2025-01-23 15:40_

## Summary

Fixes #9663 and also improves the fixes for [RUF055](https://docs.astral.sh/ruff/rules/unnecessary-regular-expression/) since regular expressions are often written as raw strings.

This doesn't include raw f-strings, but I could work on that here if we wanted.

## Test Plan

Existing snapshots for RUF055 and PT009, plus a new `Generator` test and a regression test for the reported `PIE810` issue.


---

_Label `bug` added by @ntBre on 2025-01-23 15:40_

---

_Label `fixes` added by @ntBre on 2025-01-23 15:40_

---

_@MichaReiser reviewed on 2025-01-23 15:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT009.snap`:3 on 2025-01-23 15:45_

You may have to update your `cargo insta` version so that it avoids adding `snapshot_kind` :)

---

_@MichaReiser approved on 2025-01-23 15:46_

Sweet

---

_@ntBre reviewed on 2025-01-23 15:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT009.snap`:3 on 2025-01-23 15:59_

Ahhh, you're right. Fixing now!

---

_Comment by @github-actions[bot] on 2025-01-23 16:08_

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

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1293 on 2025-01-23 17:04_

we might want to also consider using `flags.quote_str()` rather than `self.quote` here? But not for this PR -- seems like a larger change, a more invasive change, and separate to the problems being solved here

---

_@AlexWaygood approved on 2025-01-23 17:05_

Nice!

---

_@ntBre reviewed on 2025-01-23 17:09_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1293 on 2025-01-23 17:09_

Thanks for pointing this out. I'm looking at quoting next!

---

_Merged by @ntBre on 2025-01-23 17:12_

---

_Closed by @ntBre on 2025-01-23 17:12_

---

_Branch deleted on 2025-01-23 17:12_

---
