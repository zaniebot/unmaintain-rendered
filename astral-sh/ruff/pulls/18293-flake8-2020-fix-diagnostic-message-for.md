```yaml
number: 18293
title: "[`flake8-2020`] Fix diagnostic message for `!=` comparisons (`YTT201`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - diagnostics
  - codex
assignees: []
merged: true
base: main
head: codex/fix-misleading-diagnostic-for-!=-expressions-in-ytt201
created_at: 2025-05-25T00:33:54Z
updated_at: 2025-05-26T05:20:44Z
url: https://github.com/astral-sh/ruff/pull/18293
synced_at: 2026-01-12T15:56:16Z
```

# [`flake8-2020`] Fix diagnostic message for `!=` comparisons (`YTT201`)

---

_@charliermarsh_

## Summary

Closes #18292.

---

_Label `codex` added by @charliermarsh on 2025-05-25 00:33_

---

_Comment by @github-actions[bot] on 2025-05-25 00:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs`:92 on 2025-05-25 10:20_

I'd prefer a boolean saying if it's `==` or `!=`. Took me a while to realise that the rule doesn't support other compare operators

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs`:260 on 2025-05-25 10:20_

nit: use `operator @` in the let arm instead of retrieving it form `ops[0]`

---

_@MichaReiser reviewed on 2025-05-25 10:21_

How can I get chatgpt to address my comments?

---

_Label `diagnostics` added by @MichaReiser on 2025-05-25 10:21_

---

_Comment by @charliermarsh on 2025-05-25 15:43_

You can't, it doesn't support any kind of iteration.

---

_@charliermarsh reviewed on 2025-05-25 17:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs`:92 on 2025-05-25 17:09_

Changed!

---

_@charliermarsh reviewed on 2025-05-25 17:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs`:260 on 2025-05-25 17:09_

Changed!

---

_Merged by @charliermarsh on 2025-05-25 17:16_

---

_Closed by @charliermarsh on 2025-05-25 17:16_

---

_Branch deleted on 2025-05-25 17:16_

---

_Renamed from "Fix YTT201 for '!=' comparisons" to "[`flake8-2020`] Fix diagnostic message for `!=` comparisons (`YTT201`)" by @dhruvmanila on 2025-05-26 05:20_

---
