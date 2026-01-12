```yaml
number: 21309
title: "[`pyupgrade`] Fix false positive on relative imports from local `.builtins` module (`UP029`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-21307
created_at: 2025-11-06T22:57:12Z
updated_at: 2025-11-07T17:00:00Z
url: https://github.com/astral-sh/ruff/pull/21309
synced_at: 2026-01-12T15:57:20Z
```

# [`pyupgrade`] Fix false positive on relative imports from local `.builtins` module (`UP029`)

---

_@danparizher_

## Summary

Fixes false positive where `UP029` incorrectly flagged relative imports from local modules named `builtins` (e.g., `from .builtins import next`) as unnecessary builtin imports. The rule should only flag imports from Python's standard `builtins` module, not from local modules.

Fixes #21307

## Problem Analysis

The `UP029` rule checks for unnecessary imports from Python's `builtins` module. However, it was incorrectly flagging relative imports like `from .builtins import next` as unnecessary, even though these are importing from a local module named `builtins`, not Python's builtins module.

The bug occurred because the rule only checked if the module name matched `"builtins"` without distinguishing between:
- Absolute imports: `from builtins import next` (should be flagged)
- Relative imports: `from .builtins import next` (should NOT be flagged)

This caused issues in projects like `aioitertools` that reimplement builtins as async-compatible variants in a local `.builtins` module.

## Approach

The fix adds a check for relative imports by:
1. Adding a `level: u32` parameter to the `unnecessary_builtin_import` function to detect import level
2. Adding an early return when `level > 0` (indicating a relative import) before checking the module name
3. Updating the call site to pass the `level` parameter from the AST node

This ensures that relative imports are skipped entirely, as they're importing from local modules, not Python's builtins. Absolute imports continue to work correctly and are still flagged as expected.

The fix is minimal and surgical - it only adds the relative import check without modifying any existing logic for absolute imports.

---

_Comment by @github-actions[bot] on 2025-11-06 23:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@amyreese approved on 2025-11-07 16:00_

---

_Review requested from @ntBre by @amyreese on 2025-11-07 16:00_

---

_Label `rule` added by @MichaReiser on 2025-11-07 16:01_

---

_Merged by @MichaReiser on 2025-11-07 16:01_

---

_Closed by @MichaReiser on 2025-11-07 16:01_

---

_Branch deleted on 2025-11-07 17:00_

---
