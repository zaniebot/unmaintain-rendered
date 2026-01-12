```yaml
number: 7428
title: Avoid extra parentheses in unary expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/unary
created_at: 2023-09-16T02:16:36Z
updated_at: 2023-09-16T17:07:39Z
url: https://github.com/astral-sh/ruff/pull/7428
synced_at: 2026-01-12T02:39:10Z
```

# Avoid extra parentheses in unary expressions

---

_Pull request opened by @charliermarsh on 2023-09-16 02:16_

## Summary

This PR applies a similar fix to unary expressions as in https://github.com/astral-sh/ruff/pull/7424. Specifically, we only need to parenthesize the entire operator if the operand itself doesn't have parentheses, and requires parentheses.

Closes https://github.com/astral-sh/ruff/issues/7423.

## Test Plan

`cargo test`

No change in similarity.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99923 |               648 |                18 |
| zulip        |           0.99962 |              1437 |                22 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99923 |               648 |                18 |
| zulip        |           0.99962 |              1437 |                22 |


---

_Label `formatter` added by @charliermarsh on 2023-09-16 02:16_

---

_@MichaReiser approved on 2023-09-16 14:36_

---

_Comment by @MichaReiser on 2023-09-16 14:36_

Please double check that this isn't regressing the similarity index for our test-projects and update the PR summary with the metrics.

---

_Merged by @charliermarsh on 2023-09-16 17:07_

---

_Closed by @charliermarsh on 2023-09-16 17:07_

---

_Branch deleted on 2023-09-16 17:07_

---
