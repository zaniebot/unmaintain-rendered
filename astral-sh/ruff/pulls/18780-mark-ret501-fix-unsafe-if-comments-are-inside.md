```yaml
number: 18780
title: "Mark `RET501` fix unsafe if comments are inside"
type: pull_request
state: merged
author: njhearp
labels:
  - fixes
assignees: []
merged: true
base: main
head: RET501-unsafe-comment
created_at: 2025-06-19T03:00:00Z
updated_at: 2025-06-19T09:12:12Z
url: https://github.com/astral-sh/ruff/pull/18780
synced_at: 2026-01-12T15:56:25Z
```

# Mark `RET501` fix unsafe if comments are inside

---

_@njhearp_

## Summary

Fixes #18774

---

_@charliermarsh reviewed on 2025-06-19 03:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_return/RET501.py`:61 on 2025-06-19 03:00_

```suggestion
        )

```

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:57 on 2025-06-19 03:02_

```suggestion
/// This rule's fix is marked as unsafe for cases in which comments would be
/// dropped from the `return` statement.
```

---

_@charliermarsh reviewed on 2025-06-19 03:02_

---

_@charliermarsh approved on 2025-06-19 03:02_

---

_Comment by @github-actions[bot] on 2025-06-19 03:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-06-19 09:11_

---

_Merged by @MichaReiser on 2025-06-19 09:12_

---

_Closed by @MichaReiser on 2025-06-19 09:12_

---
