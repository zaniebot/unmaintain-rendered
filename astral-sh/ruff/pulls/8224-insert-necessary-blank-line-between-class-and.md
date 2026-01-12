```yaml
number: 8224
title: Insert necessary blank line between class and leading comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/blank
created_at: 2023-10-25T18:58:42Z
updated_at: 2023-10-26T00:32:01Z
url: https://github.com/astral-sh/ruff/pull/8224
synced_at: 2026-01-12T15:55:25Z
```

# Insert necessary blank line between class and leading comments

---

_@charliermarsh_

## Summary

Given:

```python
# comment

class A:
    def foo(self):
        pass
```

We need to insert an additional newline between `# comment` and `class A`. We were missing this handling for the case in which `# comment` is a leading comment on `class A`, as opposed to a trailing comment of some preceding statement.

In practice, I think this only applies to the specific case in which a class or function is the first statement in a module, and there's a single empty line between a leading comment and that class or function. If there are no empty lines, then the comment "sticks" to the definition; if there are two or more, then `leading_comments` will truncate appropriately. If the class or function is nested, then we only need one empty line anyway.

Closes https://github.com/astral-sh/ruff/issues/8215.

## Test Plan

No change in similarity.

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
| cpython        |           0.75803 |              1799 |              1648 |
| django         |           0.99983 |              2772 |                34 |
| home-assistant |           0.99953 |             10596 |               186 |
| poetry         |           0.99891 |               317 |                17 |
| transformers   |           0.99966 |              2657 |               330 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |


---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-25 19:01_

---

_Review requested from @konstin by @charliermarsh on 2023-10-25 19:01_

---

_Label `bug` added by @charliermarsh on 2023-10-25 19:01_

---

_Label `formatter` added by @charliermarsh on 2023-10-25 19:01_

---

_@MichaReiser approved on 2023-10-26 00:23_

---

_Merged by @charliermarsh on 2023-10-26 00:32_

---

_Closed by @charliermarsh on 2023-10-26 00:32_

---

_Branch deleted on 2023-10-26 00:32_

---
