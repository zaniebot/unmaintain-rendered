```yaml
number: 1848
title: "B024 and B027 false positives when using `@abc.abstractproperty`"
type: issue
state: closed
author: nsoranzo
labels: []
assignees: []
created_at: 2023-01-13T13:23:12Z
updated_at: 2023-01-14T11:44:04Z
url: https://github.com/astral-sh/ruff/issues/1848
synced_at: 2026-01-10T11:09:44Z
```

# B024 and B027 false positives when using `@abc.abstractproperty`

---

_Issue opened by @nsoranzo on 2023-01-13 13:23_

- A minimal code snippet that reproduces the bug:

```python
import abc


class Foo(metaclass=abc.ABCMeta):
    @abc.abstractproperty
    def p(self):
        """A property of Foo"""


class Bar(metaclass=abc.ABCMeta):
    @property
    @abc.abstractmethod
    def p(self):
        """A property of Foo"""
```

In the example above `Foo` and `Bar` are equivalent classes, with `Foo` using the (deprecated but still supported and widely used) [@abc.abstractproperty](https://docs.python.org/3/library/abc.html#abc.abstractproperty) decorator.

- The command invoked:

```
$ ruff --isolated --extend-select "B" test.py 
test.py:4:1: B024 `Foo` is an abstract base class, but it has no abstract methods
test.py:6:5: B027 `Foo` is an empty method in an abstract base class, but has no abstract decorator
Found 2 error(s).
```

- Expected result: no errors

- The current Ruff version (`ruff --version`): 0.0.220


---

_Closed by @charliermarsh on 2023-01-13 16:46_

---

_Comment by @nsoranzo on 2023-01-14 11:44_

Thanks for the quick fix!

---
