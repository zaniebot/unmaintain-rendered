```yaml
number: 7575
title: Allow up to two newlines before trailing clause body comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/newlines
created_at: 2023-09-21T14:42:58Z
updated_at: 2023-09-21T14:53:01Z
url: https://github.com/astral-sh/ruff/pull/7575
synced_at: 2026-01-12T15:55:24Z
```

# Allow up to two newlines before trailing clause body comments

---

_@charliermarsh_

## Summary

This is the peer to https://github.com/astral-sh/ruff/pull/7557, but for "leading" clause comments, like:

```python
if True:
    pass


# comment
else:
    pass
```

In this case, we again want to allow up to two newlines at the top level.

## Test Plan

`cargo test`

No changes.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               323 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               323 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Label `formatter` added by @charliermarsh on 2023-09-21 14:43_

---

_Comment by @charliermarsh on 2023-09-21 14:52_

(Merging, this is minor.)

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/format.rs`:87 on 2023-09-21 14:52_

is that comment still correct?

---

_Merged by @charliermarsh on 2023-09-21 14:52_

---

_Closed by @charliermarsh on 2023-09-21 14:52_

---

_Branch deleted on 2023-09-21 14:52_

---

_@konstin approved on 2023-09-21 14:53_

---
