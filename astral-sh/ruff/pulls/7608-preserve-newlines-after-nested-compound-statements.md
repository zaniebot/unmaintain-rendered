```yaml
number: 7608
title: Preserve newlines after nested compound statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/if-else
created_at: 2023-09-22T18:02:39Z
updated_at: 2025-09-07T03:24:46Z
url: https://github.com/astral-sh/ruff/pull/7608
synced_at: 2026-01-12T15:55:24Z
```

# Preserve newlines after nested compound statements

---

_@charliermarsh_

## Summary

Given:
```python
if True:
    if True:
        pass
    else:
        pass
        # a

        # b
        # c

else:
    pass
```

We want to preserve the newline after the `# c` (before the `else`). However, the `last_node` ends at the `pass`, and the comments are trailing comments on the `pass`, not trailing comments on the `last_node` (the `if`). As such, when counting the trailing newlines on the outer `if`, we abort as soon as we see the comment (`# a`).

This PR changes the logic to skip _all_ comments (even those with newlines between them). This is safe as we know that there are no "leading" comments on the `else`, so there's no risk of skipping those accidentally.

Closes https://github.com/astral-sh/ruff/issues/7602.

## Test Plan

No change in compatibility.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-22 18:02_

---

_Review requested from @konstin by @charliermarsh on 2023-09-22 18:02_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 18:02_

---

_Comment by @github-actions[bot] on 2023-09-22 18:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser approved on 2023-09-25 07:22_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:277 on 2023-09-25 08:24_

```suggestion
                // It's necessary to skip any trailing line comment because our parser doesn't
```

---

_@konstin approved on 2023-09-25 08:26_

---

_Merged by @charliermarsh on 2023-09-25 14:21_

---

_Closed by @charliermarsh on 2023-09-25 14:21_

---

_Branch deleted on 2023-09-25 14:21_

---
