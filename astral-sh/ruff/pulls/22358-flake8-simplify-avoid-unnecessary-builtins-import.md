```yaml
number: 22358
title: "flake8-simplify: avoid unnecessary builtins import for SIM105"
type: pull_request
state: merged
author: Jkhall81
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-sim105-builtin-import
created_at: 2026-01-03T15:51:50Z
updated_at: 2026-01-05T09:58:46Z
url: https://github.com/astral-sh/ruff/pull/22358
synced_at: 2026-01-10T16:36:19Z
```

# flake8-simplify: avoid unnecessary builtins import for SIM105

---

_Pull request opened by @Jkhall81 on 2026-01-03 15:51_

## Summary

This PR fixes #22334 by updating the `SIM105` fix to use `get_or_import_builtin_symbol` for `BaseException`. 

Previously, the fix would always insert an unnecessary `import builtins` and use `builtins.BaseException`. This change ensures that `BaseException` is accessed directly as a builtin, only falling back to an explicit import if the name is shadowed.

## Test Plan

- Executed the specific rule test suite: `cargo test -p ruff_linter rule_suppressibleexception`
- Verified the fix via snapshot diffs using `cargo insta review`.
- All 5 existing `SIM105` tests passed after updating snapshots.

---

_Label `fixes` added by @AlexWaygood on 2026-01-03 16:00_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 16:08_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser approved on 2026-01-05 09:58_

Thank you

---

_Merged by @MichaReiser on 2026-01-05 09:58_

---

_Closed by @MichaReiser on 2026-01-05 09:58_

---
