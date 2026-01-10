```yaml
number: 14525
title: "B039 considers `frozenset` to be mutable"
type: issue
state: closed
author: layday
labels:
  - bug
assignees: []
created_at: 2024-11-22T11:13:52Z
updated_at: 2024-11-24T02:29:26Z
url: https://github.com/astral-sh/ruff/issues/14525
synced_at: 2026-01-10T11:09:56Z
```

# B039 considers `frozenset` to be mutable

---

_Issue opened by @layday on 2024-11-22 11:13_

```
$ cat foo.py 
from contextvars import ContextVar

foo = ContextVar('foo', default=frozenset[str]())
$ ruff check --select B039 foo.py
foo.py:3:33: B039 Do not use mutable data structures for `ContextVar` defaults
  |
1 | from contextvars import ContextVar
2 | 
3 | foo = ContextVar('foo', default=frozenset[str]())
  |                                 ^^^^^^^^^^^^^^^^ B039
  |
  = help: Replace with `None`; initialize with `.set()``

Found 1 error.
$ ruff --version                 
ruff 0.8.0
```


---

_Comment by @layday on 2024-11-22 11:17_

Also appears to be the case for `tuple`, and only if they are parametrised, i.e. `frozenset()` is accepted but not `frozenset[type]()`.

---

_Comment by @layday on 2024-11-22 11:23_

Conversely, the error is silenced if the context var is an *annotated* mutable type:

```
$ cat foo.py
from contextvars import ContextVar

foo = ContextVar[set[str]]('foo', default=set())  # No error
bar = ContextVar('bar', default=set())
$ ruff check --select B039 foo.py
foo.py:4:33: B039 Do not use mutable data structures for `ContextVar` defaults
  |
3 | foo = ContextVar[set[str]]('foo', default=set())  # No error
4 | bar = ContextVar('bar', default=set())
  |                                 ^^^^^ B039
  |
  = help: Replace with `None`; initialize with `.set()``

Found 1 error.
```

---

_Comment by @layday on 2024-11-22 11:29_

To recap:

```py
from contextvars import ContextVar


# All of these should produce an error but foo3 and foo4 don't.

foo1 = ContextVar('foo1', default=set())
foo2 = ContextVar('foo2', default=set[object]())
foo3 = ContextVar[set[object]]('foo3', default=set())
foo4 = ContextVar[set[object]]('foo4', default=set[object]())
foo5: ContextVar[set[object]] = ContextVar('foo5', default=set())
foo6: ContextVar[set[object]] = ContextVar('foo6', default=set[object]())


# None of these should produce an error but bar2 and bar6 do.

bar1 = ContextVar('bar1', default=frozenset())
bar2 = ContextVar('bar2', default=frozenset[object]())
bar3 = ContextVar[frozenset[object]]('bar3', default=frozenset())
bar4 = ContextVar[frozenset[object]]('bar4', default=frozenset[object]())
bar5: ContextVar[frozenset[object]] = ContextVar('bar5', default=frozenset())
bar6: ContextVar[frozenset[object]] = ContextVar('bar6', default=frozenset[object]())
```

---

_Label `bug` added by @AlexWaygood on 2024-11-22 11:48_

---

_Comment by @harupy on 2024-11-22 13:33_

@AlexWaygood can I work on this?

---

_Comment by @AlexWaygood on 2024-11-22 13:34_

@harupy sure!

---

_Comment by @AlexWaygood on 2024-11-22 13:35_

@harupy this function will probably be helpful in fixing some of the issues here: https://github.com/astral-sh/ruff/blob/b80de52592f7b4c9854a454e2b7a0121fe2862e4/crates/ruff_python_ast/src/helpers.rs#L660-L670

---

_Assigned to @harupy by @AlexWaygood on 2024-11-22 13:35_

---

_Comment by @harupy on 2024-11-22 13:39_

@AlexWaygood Thanks!

---

_Comment by @harupy on 2024-11-22 14:19_

Filed #14532

---

_Closed by @charliermarsh on 2024-11-24 02:29_

---
