```yaml
number: 8100
title: "Respect parenthesized generators in `has_own_parentheses`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/generator
created_at: 2023-10-20T21:11:03Z
updated_at: 2023-10-22T23:58:26Z
url: https://github.com/astral-sh/ruff/pull/8100
synced_at: 2026-01-12T02:32:41Z
```

# Respect parenthesized generators in `has_own_parentheses`

---

_Pull request opened by @charliermarsh on 2023-10-20 21:11_

## Summary

When analyzing:

```python
if "root" not in (
    long_tree_name_tree.split("/")[0]
    for long_tree_name_tree in really_really_long_variable_name
):
    msg = "Could not find root. Please try a different forest."
    raise ValueError(msg)
```

We missed that the generator expression is parenthesized, because the parentheses are _part_ of the generator -- so `is_expression_parenthesized` returns `False`. We needed to take into account that generators and tuples may or may not be parenthesized when determining whether we can omit parentheses while splitting an expression.

Closes https://github.com/astral-sh/ruff/issues/8090.

## Test Plan

No changes in similarity.

Before:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75803 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                34 |
| home-assistant |           0.99953 |             10596 |               186 |
| poetry         |           0.99891 |               317 |                17 |
| transformers   |           0.99966 |              2657 |               330 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |

After:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75803 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                34 |
| home-assistant |           0.99953 |             10596 |               186 |
| poetry         |           0.99891 |               317 |                17 |
| transformers   |           0.99966 |              2657 |               330 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |


---

_@charliermarsh reviewed on 2023-10-20 21:11_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:227 on 2023-10-20 21:11_

An empty tuple has to be parenthesized. Otherwise, it... can't exist?

---

_@charliermarsh reviewed on 2023-10-20 21:11_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:515 on 2023-10-20 21:11_

I think this was a typo, but please correct me if I'm wrong.

---

_Label `bug` added by @charliermarsh on 2023-10-20 21:11_

---

_Label `formatter` added by @charliermarsh on 2023-10-20 21:11_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-20 21:11_

---

_Review requested from @konstin by @charliermarsh on 2023-10-20 21:11_

---

_@MichaReiser approved on 2023-10-22 23:46_

---

_Merged by @charliermarsh on 2023-10-22 23:58_

---

_Closed by @charliermarsh on 2023-10-22 23:58_

---

_Branch deleted on 2023-10-22 23:58_

---
