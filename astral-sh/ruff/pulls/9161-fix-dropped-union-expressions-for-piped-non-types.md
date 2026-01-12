```yaml
number: 9161
title: "Fix dropped union expressions for piped non-types in `PYI055` autofix"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-pyi055
created_at: 2023-12-16T12:14:17Z
updated_at: 2023-12-16T20:58:32Z
url: https://github.com/astral-sh/ruff/pull/9161
synced_at: 2026-01-12T15:55:27Z
```

# Fix dropped union expressions for piped non-types in `PYI055` autofix

---

_@diceroll123_

## Summary

Fix dropped union expressions for piped non-types in `PYI055` autofix

If you had `type[int] | type[str] | str`, it would have dropped the `str`, which breaks the type!

Closes #9156 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-12-16 12:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Fix dropped union expressions for piped non-types in `PYI055`" to "Fix dropped union expressions for piped non-types in `PYI055` autofix" by @diceroll123 on 2023-12-16 13:20_

---

_@charliermarsh reviewed on 2023-12-16 16:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:148 on 2023-12-16 16:02_

Do we not need to take the `other_exprs` into account in the top part of this `if`-`else`?

---

_@diceroll123 reviewed on 2023-12-16 16:03_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:148 on 2023-12-16 16:03_

I'll double-check!

---

_@charliermarsh approved on 2023-12-16 20:58_

Thank you!

---

_Merged by @charliermarsh on 2023-12-16 20:58_

---

_Closed by @charliermarsh on 2023-12-16 20:58_

---

_Label `bug` added by @charliermarsh on 2023-12-16 20:58_

---
