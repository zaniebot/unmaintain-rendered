```yaml
number: 11423
title: Clearly indicate what is counted as a branch
type: pull_request
state: merged
author: jaap3
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-05-14T11:21:18Z
updated_at: 2024-05-16T02:18:52Z
url: https://github.com/astral-sh/ruff/pull/11423
synced_at: 2026-01-12T15:55:38Z
```

# Clearly indicate what is counted as a branch

---

_@jaap3_

## Summary

As discussed in issue #11408, PLR0912 has a broader definition of "branches" than I expected. This updates the documentation to include this definition.

I also updated the example to include several different types of branches, while still maintaining dictionary lookup as an alternative solution. (Crafting a realistic example was quite a challenge 😅).

Closes https://github.com/astral-sh/ruff/issues/11408.

---

_Comment by @github-actions[bot] on 2024-05-14 11:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:19 on 2024-05-14 12:00_

Can we retain the old example as a simple first case? It can be in a separate code block e.g.

Instead of 

```
ex1
```

use

```
ex1
```

Instead of

```
ex2
```

use

```
ex2
```



---

_@zanieb reviewed on 2024-05-14 12:00_

---

_@jaap3 reviewed on 2024-05-14 12:40_

---

_Review comment by @jaap3 on `crates/ruff_linter/src/rules/pylint/rules/too_many_branches.rs`:19 on 2024-05-14 12:40_

Done (I'm using the GitHub editor, so please squash these useless commits on merge)

---

_Label `documentation` added by @charliermarsh on 2024-05-16 02:05_

---

_@charliermarsh approved on 2024-05-16 02:05_

---

_Merged by @charliermarsh on 2024-05-16 02:17_

---

_Closed by @charliermarsh on 2024-05-16 02:17_

---
