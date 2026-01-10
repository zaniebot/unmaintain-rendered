```yaml
number: 10897
title: Add tests for simple target in annotated assign stmt
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
  - fuzzer
assignees: []
merged: true
base: dhruv/parser
head: dhruv/ann-assign-simple-target-test
created_at: 2024-04-12T06:28:38Z
updated_at: 2024-04-12T06:49:42Z
url: https://github.com/astral-sh/ruff/pull/10897
synced_at: 2026-01-10T22:37:01Z
```

# Add tests for simple target in annotated assign stmt

---

_Pull request opened by @dhruvmanila on 2024-04-12 06:28_

> `simple` is a boolean integer set to `True` for a [Name](https://docs.python.org/3/library/ast.html#ast.Name) node in `target` that do not appear in between parenthesis and are hence pure names and not expressions.

Reference: https://docs.python.org/3/library/ast.html#ast.AnnAssign

---

_Label `bug` added by @dhruvmanila on 2024-04-12 06:28_

---

_Label `parser` added by @dhruvmanila on 2024-04-12 06:28_

---

_Label `fuzzer` added by @dhruvmanila on 2024-04-12 06:28_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-12 06:28_

---

_Merged by @dhruvmanila on 2024-04-12 06:28_

---

_Closed by @dhruvmanila on 2024-04-12 06:28_

---

_Branch deleted on 2024-04-12 06:28_

---

_Comment by @github-actions[bot] on 2024-04-12 06:49_

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
