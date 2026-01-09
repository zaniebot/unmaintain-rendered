---
number: 8579
title: "new rule - not calling super() in custom exception's __init__()"
type: issue
state: open
author: m-aciek
labels:
  - rule
assignees: []
created_at: 2023-11-09T11:18:54Z
updated_at: 2025-06-11T11:41:13Z
url: https://github.com/astral-sh/ruff/issues/8579
synced_at: 2026-01-07T13:12:15-06:00
---

# new rule - not calling super() in custom exception's __init__()

---

_Issue opened by @m-aciek on 2023-11-09 11:18_

```python
class CustomException(Exception):
    def __init__(self, custom: int, arguments: str) -> None:
        self.msg = f"Something specific happened with custom: {custom} and {arguments}"
```

Above exception class has an issue, in my personal experience it's quite common. The implementer assumes, that they will use the `self.msg` parameter during exception handling. The issue araises when the exception gets anywhere else as the its string representation is as follows:

```python
>>> raise CustomException(42, 'test')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.CustomException: (42, 'test')
```

and when the exception is instantiated with keyword arguments, we lose the information about the arguments' values.

```python
>>> raise CustomException(custom=1, arguments='two')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.CustomException
```

How to fix it? By calling `super().__init__()` in the constructor. That way the string representation of the custom exception will contain the error message.

```python
class CustomException(Exception):
    def __init__(self, custom: int, arguments: str) -> None:
        self.msg = "Something specific happened with custom: {custom} and {arguments}"
        super().__init__(self.msg)
>>> raise CustomException(1, 'two')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.CustomException: Something specific happened with custom: 1 and two
>>> raise CustomException(custom=1, parameters='two')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.CustomException: Something specific happened with custom: 1 and two
```

**Specification proposal**

If a class:
* inherits from from `Exception`,
* doesn't implement own `__str__()` method,
* its `__init__()` method doesn't enforce any positional-only argument, 
* and doesn't call `super().__init__()` in its `__init__()` method

Then:
* new rule should raise an error, calling to fix it.

---

_Label `rule` added by @charliermarsh on 2023-11-10 05:33_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-10 05:33_

---

_Comment by @m-aciek on 2023-11-16 02:25_

For the reference, this has been accepted in [`flake8-bugbear`](https://github.com/PyCQA/flake8-bugbear/issues/421).

---

_Label `needs-decision` removed by @zanieb on 2023-11-16 02:28_

---

_Comment by @opk12 on 2025-04-24 17:51_

Related: linting for a missing `super` call in *any* `__init__()` (not only inside exception classes) is tracked in #970 (`implement Pylint`) as the list item `super-init-not-called / W0231`.

---

_Comment by @jakkdl on 2025-06-11 11:41_

https://github.com/PyCQA/flake8-bugbear/pull/512 has now been merged upstream

---
