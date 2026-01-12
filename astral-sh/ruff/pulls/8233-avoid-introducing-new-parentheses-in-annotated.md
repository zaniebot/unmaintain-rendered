```yaml
number: 8233
title: Avoid introducing new parentheses in annotated assignments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/ann-assign
created_at: 2023-10-25T22:10:03Z
updated_at: 2023-10-26T02:51:51Z
url: https://github.com/astral-sh/ruff/pull/8233
synced_at: 2026-01-12T15:55:25Z
```

# Avoid introducing new parentheses in annotated assignments

---

_@charliermarsh_

## Summary

We decided to avoid changing this in https://github.com/astral-sh/ruff/issues/7315, but it's been reported multiple times (e.g., in https://github.com/astral-sh/ruff/issues/8226, also on Discord). I suggest we change it to improve compatibility. In general, it also seems to lend itself to better code style.

Closes #8188 
Closes #8226

## Test Plan

Shows improvements for CPython, home-assistant, Poetry, and typeshed.

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
| cpython        |           0.75804 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                34 |
| home-assistant |           0.99960 |             10596 |               156 |
| poetry         |           0.99897 |               317 |                17 |
| transformers   |           0.99966 |              2657 |               330 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |


---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-25 22:11_

---

_Review requested from @konstin by @charliermarsh on 2023-10-25 22:11_

---

_Label `formatter` added by @charliermarsh on 2023-10-25 23:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_ann_assign.rs`:24 on 2023-10-25 23:57_

```suggestion
            [target.format(), token(":"), space(), annotation.format()]
```

---

_@MichaReiser reviewed on 2023-10-26 00:03_

We should update the documentation too. 

Are there type annotations in other positions that need updating too? 

I'm somewhat torn on this (see comment in #8188) because I think the unfortunate formatting will be fixed when implementing the preview style. Also, there are now cases (with unions on the left), that now exceed the line width, but didn't before. Should we special case instead to only avoid parenthesizing if the type annotation is a single name?

Could we gate the old behavior behind the preview flag? I think the old formatting yields better results if we parenthesize the left, if parenthesizing the right didn't make it fit. 

---

_Comment by @charliermarsh on 2023-10-26 01:49_

Went ahead and updated the docs. My current thinking here is that we should change this based on improving Black compatibility. The current formatting _is_ better in some cases, e.g., from the docs:

```python
# Black
StartElementHandler: Callable[[str, dict[str, str]], Any] | Callable[[str, list[str]], Any] | Callable[
    [str, dict[str, str], list[str]], Any
] | None

# Ruff
StartElementHandler: (
    Callable[[str, dict[str, str]], Any]
    | Callable[[str, list[str]], Any]
    | Callable[[str, dict[str, str], list[str]], Any]
    | None
)
```

But I think we can improve those by making progress on preview style -- and complex union annotations are also much rarer than simple annotations.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-26 01:55_

---

_@MichaReiser approved on 2023-10-26 02:18_

---

_Merged by @charliermarsh on 2023-10-26 02:51_

---

_Closed by @charliermarsh on 2023-10-26 02:51_

---

_Branch deleted on 2023-10-26 02:51_

---
