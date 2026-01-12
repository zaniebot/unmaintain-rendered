```yaml
number: 22359
title: "pyflakes: fix F401 infinite loop via improved submodule re-export logic"
type: pull_request
state: open
author: Jkhall81
labels:
  - bug
  - rule
  - fixes
assignees: []
base: main
head: bugfix/22221-f401-infinite-loop
created_at: 2026-01-03T16:31:17Z
updated_at: 2026-01-09T08:06:48Z
url: https://github.com/astral-sh/ruff/pull/22359
synced_at: 2026-01-12T15:57:48Z
```

# pyflakes: fix F401 infinite loop via improved submodule re-export logic

---

_@Jkhall81_

## Summary

This PR resolves a convergence failure (infinite loop) in rule `F401` occurring in `__init__.py` files. 

The fix is two-fold:
1. **Linter Logic**: Updated `unused_import.rs` to recognize that a parent package listed in `__all__` correctly re-exports its submodule imports (e.g., `["foo"]` now covers `import foo.bar`).
2. **Fixer Idempotency**: Hardened `add_to_dunder_all` in `edits.rs` to check for existing entries before inserting new ones. This prevents duplicate strings from being appended and ensures the fixer terminates even if the linter re-flags an entry.

Fixes #22221

## Test Plan

- **Regression Tests**: Added new test cases to `crates/ruff_linter/src/fix/edits.rs` covering duplicate detection in both lists and tuples.
- **Snapshot Updates**: Updated existing `pyflakes` snapshots to reflect the improved submodule matching logic.
- **Manual Verification**: Verified against the reproduction case in the linked issue using `ruff check --fix --preview`.
- **Suite Verification**: Ran the full linter test suite (`cargo test -p ruff_linter`) with 2625 tests passing.

---

_@amyreese approved on 2026-01-08 18:03_

---

_Review requested from @ntBre by @amyreese on 2026-01-08 18:03_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 18:18_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review requested from @dylwil3 by @MichaReiser on 2026-01-08 18:31_

---

_Review request for @ntBre removed by @MichaReiser on 2026-01-08 18:31_

---

_Comment by @amyreese on 2026-01-08 19:25_

Looks like this needs a rebase to clean up ecosystem results.

---

_Label `bug` added by @MichaReiser on 2026-01-09 08:06_

---

_Label `rule` added by @MichaReiser on 2026-01-09 08:06_

---

_Label `rule` removed by @MichaReiser on 2026-01-09 08:06_

---

_Label `fixes` added by @MichaReiser on 2026-01-09 08:06_

---

_Label `rule` added by @MichaReiser on 2026-01-09 08:06_

---
