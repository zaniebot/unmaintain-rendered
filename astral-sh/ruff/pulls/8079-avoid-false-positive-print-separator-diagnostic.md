```yaml
number: 8079
title: Avoid false-positive print separator diagnostic with starred argument
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2023-10-19T22:11:04Z
updated_at: 2023-10-19T22:43:03Z
url: https://github.com/astral-sh/ruff/pull/8079
synced_at: 2026-01-12T15:55:25Z
```

# Avoid false-positive print separator diagnostic with starred argument

---

_@charliermarsh_

Given `print(*a_list_with_elements, sep="\n")`, we can't remove the separator (unlike in `print(a, sep="\n")`), since we don't know how many arguments were provided.

Closes https://github.com/astral-sh/ruff/issues/8078.


---

_Label `bug` added by @charliermarsh on 2023-10-19 22:11_

---

_Merged by @charliermarsh on 2023-10-19 22:30_

---

_Closed by @charliermarsh on 2023-10-19 22:30_

---

_Branch deleted on 2023-10-19 22:30_

---

_Comment by @github-actions[bot] on 2023-10-19 22:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
