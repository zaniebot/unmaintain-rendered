---
number: 12062
title: "Bug: PLR1714 auto-fix with multiple conditions"
type: issue
state: closed
author: Spectre5
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-06-27T04:28:33Z
updated_at: 2024-07-17T15:49:28Z
url: https://github.com/astral-sh/ruff/issues/12062
synced_at: 2026-01-10T01:22:51Z
---

# Bug: PLR1714 auto-fix with multiple conditions

---

_Issue opened by @Spectre5 on 2024-06-27 04:28_

Ruff removes the `a == 1` condition in the example below when applying the (unsafe) auto-fix.

Consider this source as test.py:

```python
def PLR1714_demo():
    a = 1
    b = '2'
    if a == 1 or b == '2' or b == '3':
        pass
```

Then:

```console
$ ruff --version
ruff 0.4.10
$ ruff check --isolated --select PLR1714 test.py 
test.py:4:8: PLR1714 Consider merging multiple comparisons: `b in ('2', '3')`. Use a `set` if the elements are hashable.
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
$ ruff check --isolated --select PLR1714 --fix --unsafe-fixes test.py 
Found 1 error (1 fixed, 0 remaining).
```

Now the contents of test.py is shown below, where the conditional `a == 1` has been removed.

```python
def PLR1714_demo():
    a = 1
    b = '2'
    if b in ('2', '3'):
        pass
```

---

_Comment by @dhruvmanila on 2024-06-27 05:30_

I think this is similar to the bug I found for `PLR1701` (which has been removed and redirected to use `SIM101`) as seen in the [PR description](https://github.com/astral-sh/ruff/pull/12021).

---

_Label `bug` added by @dhruvmanila on 2024-06-27 05:30_

---

_Label `fixes` added by @charliermarsh on 2024-06-27 18:48_

---

_Comment by @charliermarsh on 2024-06-27 18:48_

Thanks, we'll fix!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 14:34_

---

_Referenced in [astral-sh/ruff#12368](../../astral-sh/ruff/pulls/12368.md) on 2024-07-17 15:19_

---

_Closed by @charliermarsh on 2024-07-17 15:49_

---

_Closed by @charliermarsh on 2024-07-17 15:49_

---
