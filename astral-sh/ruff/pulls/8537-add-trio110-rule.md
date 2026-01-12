```yaml
number: 8537
title: Add TRIO110 rule
type: pull_request
state: merged
author: karpetrosyan
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-trio-110-suppor
created_at: 2023-11-07T12:23:41Z
updated_at: 2023-11-07T22:13:19Z
url: https://github.com/astral-sh/ruff/pull/8537
synced_at: 2026-01-12T15:55:26Z
```

# Add TRIO110 rule

---

_@karpetrosyan_

## Summary

Adds TRIO110 from the [flake8-trio plugin](https://github.com/Zac-HD/flake8-trio).
Relates to: https://github.com/astral-sh/ruff/issues/8451

---

_Review comment by @karpetrosyan on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1221 on 2023-11-07 12:24_

```suggestion
```

---

_@karpetrosyan reviewed on 2023-11-07 12:24_

---

_Comment by @github-actions[bot] on 2023-11-07 12:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@karpetrosyan reviewed on 2023-11-07 12:47_

---

_Review comment by @karpetrosyan on `crates/ruff_linter/src/rules/flake8_trio/rules/unneeded_sleep.rs`:13 on 2023-11-07 12:47_

```suggestion
/// it's preferable to use a `trio.Event`.
```

---

_@charliermarsh approved on 2023-11-07 21:06_

Nice, thank you!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/unneeded_sleep.rs`:44 on 2023-11-07 21:06_

In general, we prefer passing in the typed node (`ast::StmtWhile` instead of the individual properties of it, like `body`). That way, users can't accidentally pass an AST node of the wrong type.

---

_@charliermarsh reviewed on 2023-11-07 21:06_

---

_Merged by @charliermarsh on 2023-11-07 21:27_

---

_Closed by @charliermarsh on 2023-11-07 21:27_

---

_Label `rule` added by @charliermarsh on 2023-11-07 22:13_

---

_Label `preview` added by @charliermarsh on 2023-11-07 22:13_

---
