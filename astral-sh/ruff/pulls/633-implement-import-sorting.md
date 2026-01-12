```yaml
number: 633
title: Implement import sorting
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isort
created_at: 2022-11-07T03:40:08Z
updated_at: 2024-06-12T10:51:12Z
url: https://github.com/astral-sh/ruff/pull/633
synced_at: 2026-01-12T15:55:05Z
```

# Implement import sorting

---

_@charliermarsh_

This PR implements `isort`-like import sorting behavior. The intent is to mirror `isort` when used with the following configuration:

```toml
[tool.isort]
profile = "black"
combine_as_imports = true
```

The main limitations in the current approach are as follows:

1. We still have no way of reconciling conflicting autofix errors. So if you have unused and unsorted imports, Ruff will only fix one of the two in the first pass (probably the unsorted imports, since that block comes first), and you'll have to run Ruff again to fix the other.
2. We don't have any comment-handling, so all comments are removed entirely

For now, this is implemented via an entirely separator visitor, mostly to make the code easier to reason about. I need to benchmark the impact of that choice.

Resolves: #465.


---

_Marked ready for review by @charliermarsh on 2022-11-10 23:48_

---

_Merged by @charliermarsh on 2022-11-11 00:05_

---

_Closed by @charliermarsh on 2022-11-11 00:05_

---

_Branch deleted on 2022-11-11 00:05_

---

_Comment by @nik-hil on 2024-06-12 10:50_

If somebody is wondering how to add this in pre-commit, use this
```
-   repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.2.0
    hooks:
    -   id: ruff
        args: ["check", "--select", "I", "--fix"]
    -   id: ruff-format
 ```

---
