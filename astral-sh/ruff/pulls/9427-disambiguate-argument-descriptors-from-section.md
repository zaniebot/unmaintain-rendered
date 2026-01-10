```yaml
number: 9427
title: Disambiguate argument descriptors from section headers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/nested-args
created_at: 2024-01-08T02:35:17Z
updated_at: 2024-01-08T03:41:01Z
url: https://github.com/astral-sh/ruff/pull/9427
synced_at: 2026-01-10T22:57:09Z
```

# Disambiguate argument descriptors from section headers

---

_Pull request opened by @charliermarsh on 2024-01-08 02:35_

## Summary

Given a docstring like:

```python
def func(x: int, args: tuple[int]):
    """Toggle the gizmo.

    Args:
        x: Some argument.
        args: Some other arguments.
    """
```

We were considering the `args:` descriptor to be an indented docstring section header (since `Args:`) is a valid header name. This led to very confusing diagnostics.

This PR makes the parsing a bit more lax in this case, such that if we see a nested header that's more deeply indented than the preceding header, and the preceding section allows sub-items (like `Args:`), we avoid treating the nested item as a section header.

Closes https://github.com/astral-sh/ruff/issues/9426.


---

_Label `bug` added by @charliermarsh on 2024-01-08 02:35_

---

_Label `docstring` added by @charliermarsh on 2024-01-08 02:35_

---

_Marked ready for review by @charliermarsh on 2024-01-08 02:35_

---

_Comment by @github-actions[bot] on 2024-01-08 02:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-01-08 03:41_

---

_Closed by @charliermarsh on 2024-01-08 03:41_

---

_Branch deleted on 2024-01-08 03:41_

---
