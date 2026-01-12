```yaml
number: 1500
title: Improve PLW0120 range
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: improve-PLW0120-range
created_at: 2022-12-31T07:25:06Z
updated_at: 2022-12-31T12:42:50Z
url: https://github.com/astral-sh/ruff/pull/1500
synced_at: 2026-01-12T05:36:31Z
```

# Improve PLW0120 range

---

_Pull request opened by @harupy on 2022-12-31 07:25_

This RP improves PLW0120's range.

### Current

```
resources/test/fixtures/pylint/useless_else_on_loop.py:96:9: PLW0120 Else clause on loop without a break statement, remove the else and de-indent all the code inside it
    |
 96 |           for _ in range(3):
    |  _________^
 97 | |             pass
 98 | |         else:
 99 | |             if 1 < 2:  # pylint: disable=comparison-of-constants
100 | |                 break
    | |_____________________^ PLW0120
    |
```


### Improved

```
resources/test/fixtures/pylint/useless_else_on_loop.py:98:9: PLW0120 Else clause on loop without a break statement, remove the else and de-indent all the code inside it
   |
98 |         else:
   |         ^^^^ PLW0120
   |
```


---

_@harupy reviewed on 2022-12-31 07:49_

---

_Review comment by @harupy on `src/ast/helpers.rs`:430 on 2022-12-31 07:49_

We can add support `If` and `Try` later if necessary. For `PLW0120`, supporting `For`, `AsyncFor`, and `While` should be sufficient.

---

_Merged by @charliermarsh on 2022-12-31 12:42_

---

_Closed by @charliermarsh on 2022-12-31 12:42_

---
