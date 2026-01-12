```yaml
number: 1526
title: "Fix `Name` node range in `NamedExpr` node"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-Name-range-in-NamedExpr
created_at: 2023-01-01T07:09:54Z
updated_at: 2023-01-01T07:47:26Z
url: https://github.com/astral-sh/ruff/pull/1526
synced_at: 2026-01-12T05:36:31Z
```

# Fix `Name` node range in `NamedExpr` node

---

_Pull request opened by @harupy on 2023-01-01 07:09_

https://github.com/RustPython/RustPython/pull/4391 fixed `Name` node range in `NamedExpr` node.

### Before

```
> cargo run -- --no-cache resources/test/fixtures/pycodestyle/E741.py --show-source
resources/test/fixtures/pycodestyle/E741.py:74:5: E741 Ambiguous variable name: `l`
   |
74 | if (l := 5) > 0:
   |     ^^^^^^ E741
   |
```

### After

```
> cargo run -- --no-cache resources/test/fixtures/pycodestyle/E741.py --show-source
resources/test/fixtures/pycodestyle/E741.py:74:5: E741 Ambiguous variable name: `l`
   |
74 | if (l := 5) > 0:
   |     ^ E741
   |
```

---

_Merged by @charliermarsh on 2023-01-01 07:41_

---

_Closed by @charliermarsh on 2023-01-01 07:41_

---
