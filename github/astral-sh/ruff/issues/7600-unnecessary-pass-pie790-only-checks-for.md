---
number: 7600
title: "`unnecessary-pass` (`PIE790`) only checks for docstrings immediately followed by a `pass` statement"
type: issue
state: closed
author: tjkuson
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-09-22T15:58:46Z
updated_at: 2023-09-29T01:39:13Z
url: https://github.com/astral-sh/ruff/issues/7600
synced_at: 2026-01-07T13:12:15-06:00
---

# `unnecessary-pass` (`PIE790`) only checks for docstrings immediately followed by a `pass` statement

---

_Issue opened by @tjkuson on 2023-09-22 15:58_

Currently, `PIE790` triggers on

```python
def foo():
    """A docstring."""
    pass
```

as expected. However, it does not trigger on

```python
def foo():
    print("foo")
    pass
```

or

```python
def foo():
    """A docstring."""
    print("foo")
    pass
```

which is unexpected.


---

_Comment by @dhruvmanila on 2023-09-22 17:37_

I think that's an expected behavior as per the implementation. It's detected only when a docstring is followed by the pass statement. Do you think the [documentation](https://docs.astral.sh/ruff/rules/unnecessary-pass/) was unclear regarding this? 

---

_Label `question` added by @dhruvmanila on 2023-09-22 17:37_

---

_Comment by @tjkuson on 2023-09-23 09:31_

> Do you think the [documentation](https://docs.astral.sh/ruff/rules/unnecessary-pass/) was unclear regarding this?

Yes. Perhaps it's just me, but I think the documentation suggests that it catches all unnecessary `pass` statements, with a `pass` statement immediately following a docstring as _one example_ of an unnecessary `pass` statement. If this really is intended, then I think the documentation (and the name of the rule) should be altered.

However, might there be merit to changing the implementation to catch _all_ unnecessary `pass` statements? It would be more befitting of the current rule name and probably more useful to users.

---

_Comment by @dhruvmanila on 2023-09-26 02:14_

> However, might there be merit to changing the implementation to catch _all_ unnecessary `pass` statements? It would be more befitting of the current rule name and probably more useful to users.

Yeah, I'm thinking in the same direction. I think the rule should be updated to catch all such cases.

It should be fairly simple to implement this as the function body is just a `Vec<Stmt>`, so we could just check the length of it. If the number of statements is greater than 1 and the last statement is a `Pass` statement then we can just remove it.

---

_Label `question` removed by @dhruvmanila on 2023-09-26 02:14_

---

_Label `bug` added by @dhruvmanila on 2023-09-26 02:14_

---

_Label `good first issue` added by @zanieb on 2023-09-26 17:01_

---

_Comment by @charliermarsh on 2023-09-26 23:51_

Yeah, I don't mind changing it.

---

_Referenced in [astral-sh/ruff#7697](../../astral-sh/ruff/pulls/7697.md) on 2023-09-28 14:35_

---

_Closed by @charliermarsh on 2023-09-29 01:39_

---
