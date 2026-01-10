```yaml
number: 9388
title: Ignore trailing quotes for unclosed l-brace errors
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/f-string-err
created_at: 2024-01-04T04:31:49Z
updated_at: 2024-01-04T05:11:29Z
url: https://github.com/astral-sh/ruff/pull/9388
synced_at: 2026-01-10T23:07:18Z
```

# Ignore trailing quotes for unclosed l-brace errors

---

_Pull request opened by @charliermarsh on 2024-01-04 04:31_

## Summary

Given:

```python
F"{"ڤ
```

We try to locate the "unclosed left brace" error by subtracting the quote size from the lexer offset -- so we subtract 1 from the end of the source, which puts us in the middle of a Unicode character. I don't think we should try to adjust the offset in this way, since there can be content _after_ the quote. For example, with the advent of PEP 701, this string could reasonably be fixed as:

```python
F"{"ڤ"}"
````

Closes https://github.com/astral-sh/ruff/issues/9379.


---

_Comment by @github-actions[bot] on 2024-01-04 04:48_

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

_Marked ready for review by @charliermarsh on 2024-01-04 04:55_

---

_Merged by @charliermarsh on 2024-01-04 05:00_

---

_Closed by @charliermarsh on 2024-01-04 05:00_

---

_Branch deleted on 2024-01-04 05:00_

---
