```yaml
number: 7556
title: Fix instability in trailing clause body comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/comment
created_at: 2023-09-20T23:40:15Z
updated_at: 2023-09-21T13:32:19Z
url: https://github.com/astral-sh/ruff/pull/7556
synced_at: 2026-01-12T15:55:24Z
```

# Fix instability in trailing clause body comments

---

_@charliermarsh_

## Summary

When we format the trailing comments on a clause body, we check if there are any newlines after the last statement; if not, we insert one.

This logic didn't take into account that the last statement could itself have trailing comments, as in:

```python
if True:
    pass

    # comment
else:
    pass
```

We were thus inserting a newline after the comment, like:

```python
if True:
    pass

    # comment

else:
    pass
```

In the context of function definitions, this led to an instability, since we insert a newline _after_ a function, which would in turn lead to the bug above appearing in the second formatting pass.

Closes https://github.com/astral-sh/ruff/issues/7465.

## Test Plan

`cargo test`

Small improvement in `transformers`, but no regressions.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99956 |              2587 |               404 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| **transformers** |           **0.99957** |              **2587** |               **402** |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Label `bug` added by @charliermarsh on 2023-09-20 23:40_

---

_Label `formatter` added by @charliermarsh on 2023-09-20 23:40_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-21 00:01_

---

_Review requested from @konstin by @charliermarsh on 2023-09-21 00:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:104 on 2023-09-21 06:06_

Nit: Can we us `lines_after` because we don't need to skip over comments?

---

_@MichaReiser approved on 2023-09-21 06:06_

---

_@konstin approved on 2023-09-21 08:00_

---

_Merged by @charliermarsh on 2023-09-21 13:32_

---

_Closed by @charliermarsh on 2023-09-21 13:32_

---

_Branch deleted on 2023-09-21 13:32_

---
