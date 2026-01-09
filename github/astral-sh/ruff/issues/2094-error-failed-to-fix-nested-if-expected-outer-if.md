---
number: 2094
title: "[ERROR] Failed to fix nested if: Expected outer if to have indented body and no else"
type: issue
state: closed
author: LefterisJP
labels:
  - bug
assignees: []
created_at: 2023-01-22T22:12:41Z
updated_at: 2023-01-22T22:40:58Z
url: https://github.com/astral-sh/ruff/issues/2094
synced_at: 2026-01-07T13:12:14-06:00
---

# [ERROR] Failed to fix nested if: Expected outer if to have indented body and no else

---

_Issue opened by @LefterisJP on 2023-01-22 22:12_

ruff version: `0.0.230`

```python
a = True
b = True

if a:
    if b:
        print('do something')  # noqa: T201
else:
    print('at else')  # noqa: T201
```

Running ruff here will result with:

```
test.py:4:1: SIM102 Use a single `if` statement instead of nested `if` statements
```

If we try to autofix we get `[2023-01-22][23:09:59][ruff::rules::flake8_simplify::rules::ast_if][ERROR] Failed to fix nested if: Expected outer if to have indented body and no else`

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-22 22:14_

---

_Comment by @charliermarsh on 2023-01-22 22:15_

Apart from the autofix error, this is a false positive, right?!

---

_Label `bug` added by @charliermarsh on 2023-01-22 22:15_

---

_Comment by @charliermarsh on 2023-01-22 22:17_

Looks like a regression introduced in #2050 \cc @harupy 

---

_Comment by @charliermarsh on 2023-01-22 22:20_

I'll either fix or revert that for now

---

_Comment by @LefterisJP on 2023-01-22 22:23_

> Apart from the autofix error, this is a false positive, right?!

It's a really tricky false positive.

I think if you "fix" to this code:

```python
if a and b:
    print('do something')  # noqa: T201
else:
    print('at else')  # noqa: T201
```

Then only if `a=True` and `b=False` you get different program flow. Original code would not print anything, while fixed code would go in the else.

On the other hand, the places where it showed up in our code, did require some refactoring, so I don't count it as a total loss. 

---

_Comment by @charliermarsh on 2023-01-22 22:28_

Yeah, I do consider it a false positive because the suggested fix actually has different behavior and semantics.


---

_Referenced in [astral-sh/ruff#2095](../../astral-sh/ruff/pulls/2095.md) on 2023-01-22 22:39_

---

_Comment by @charliermarsh on 2023-01-22 22:39_

Ok, this should be fixed in the next release (either tonight or tomorrow).

---

_Closed by @charliermarsh on 2023-01-22 22:40_

---

_Closed by @charliermarsh on 2023-01-22 22:40_

---
