```yaml
number: 10194
title: "Check for use of `debugpy` and `ptvsd` debug modules (#10177)"
type: pull_request
state: merged
author: jeremy-hiatt
labels:
  - rule
assignees: []
merged: true
base: main
head: add-debug-modules-to-t100
created_at: 2024-03-02T02:33:54Z
updated_at: 2024-03-02T04:02:44Z
url: https://github.com/astral-sh/ruff/pull/10194
synced_at: 2026-01-12T15:55:31Z
```

# Check for use of `debugpy` and `ptvsd` debug modules (#10177)

---

_@jeremy-hiatt_

## Summary

This addresses https://github.com/astral-sh/ruff/issues/10177.

## Test Plan

I added additional lines to the existing test file for T100.


---

_Review comment by @jeremy-hiatt on `crates/ruff_linter/src/rules/flake8_debugger/rules/debugger.rs`:113 on 2024-03-02 02:40_

I tried to pick the API methods that looked like they would be most commonly used based on the documentation:
https://github.com/microsoft/debugpy/?tab=readme-ov-file#debugpy-import-usage
https://github.com/microsoft/ptvsd?tab=readme-ov-file#ptvsd-import-usage


---

_@jeremy-hiatt reviewed on 2024-03-02 02:40_

---

_Comment by @github-actions[bot] on 2024-03-02 02:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-03-02 04:02_

This is great, thank you!

---

_@charliermarsh reviewed on 2024-03-02 04:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_debugger/rules/debugger.rs`:113 on 2024-03-02 04:02_

Spot-checked, makes sense to me.

---

_Label `rule` added by @charliermarsh on 2024-03-02 04:02_

---

_Merged by @charliermarsh on 2024-03-02 04:02_

---

_Closed by @charliermarsh on 2024-03-02 04:02_

---
