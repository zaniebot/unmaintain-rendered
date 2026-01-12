```yaml
number: 1880
title: Fix range of SIM201, 202, and 208
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: include-not
created_at: 2023-01-15T01:44:52Z
updated_at: 2023-01-15T02:19:56Z
url: https://github.com/astral-sh/ruff/pull/1880
synced_at: 2026-01-12T05:36:32Z
```

# Fix range of SIM201, 202, and 208

---

_Pull request opened by @harupy on 2023-01-15 01:44_

Before

```
resources/test/fixtures/flake8_simplify/SIM208.py:1:13: SIM208 Use `a` instead of `not (not a)`
  |
1 | if not (not a):  # SIM208
  |             ^ SIM208
  |
  = help: Replace with `a`

resources/test/fixtures/flake8_simplify/SIM208.py:4:14: SIM208 Use `a == b` instead of `not (not a == b)`
  |
4 | if not (not (a == b)):  # SIM208
  |              ^^^^^^ SIM208
  |
  = help: Replace with `a == b`
```

After

```
resources/test/fixtures/flake8_simplify/SIM208.py:1:4: SIM208 Use `a` instead of `not (not a)`
  |
1 | if not (not a):  # SIM208
  |    ^^^^^^^^^^^ SIM208
  |
  = help: Replace with `a`

resources/test/fixtures/flake8_simplify/SIM208.py:4:4: SIM208 Use `a == b` instead of `not (not a == b)`
  |
4 | if not (not (a == b)):  # SIM208
  |    ^^^^^^^^^^^^^^^^^^ SIM208
  |
  = help: Replace with `a == b`
```

---

_Comment by @harupy on 2023-01-15 01:45_

Updating snapshots now

---

_Merged by @charliermarsh on 2023-01-15 02:17_

---

_Closed by @charliermarsh on 2023-01-15 02:17_

---

_Branch deleted on 2023-01-15 02:19_

---
