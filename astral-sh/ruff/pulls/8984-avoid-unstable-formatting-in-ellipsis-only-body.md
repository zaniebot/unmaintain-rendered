```yaml
number: 8984
title: Avoid unstable formatting in ellipsis-only body with trailing comment
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/ellipsis
created_at: 2023-12-03T20:47:06Z
updated_at: 2023-12-04T00:15:42Z
url: https://github.com/astral-sh/ruff/pull/8984
synced_at: 2026-01-12T15:55:27Z
```

# Avoid unstable formatting in ellipsis-only body with trailing comment

---

_@charliermarsh_

## Summary

We should avoid inlining the ellipsis in:

```python
def h():
    ...
    # bye
```

Just as we omit the ellipsis in:

```python
def h():
    # bye
    ...
```

Closes https://github.com/astral-sh/ruff/issues/8905.

---

_Label `bug` added by @charliermarsh on 2023-12-03 20:47_

---

_Label `formatter` added by @charliermarsh on 2023-12-03 20:47_

---

_Comment by @github-actions[bot] on 2023-12-03 20:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2023-12-04 00:13_

Thank you

---

_Merged by @charliermarsh on 2023-12-04 00:15_

---

_Closed by @charliermarsh on 2023-12-04 00:15_

---

_Branch deleted on 2023-12-04 00:15_

---
