```yaml
number: 2490
title: "Synthesize the `__slots__` attribute on namedtuple classes"
type: issue
state: open
author: AlexWaygood
labels:
  - runtime semantics
assignees: []
created_at: 2026-01-14T13:06:45Z
updated_at: 2026-01-14T13:23:08Z
url: https://github.com/astral-sh/ty/issues/2490
synced_at: 2026-01-14T13:42:04Z
```

# Synthesize the `__slots__` attribute on namedtuple classes

---

_@AlexWaygood_

`NamedTuple` classes (of all kinds) have a `__slots__` attribute generated for them at runtime (it's always an empty tuple):

```pycon
>>> import collections, typing
>>> A = collections.namedtuple("A", ())
>>> B = typing.NamedTuple("B", ())
>>> class C(typing.NamedTuple): ...
... 
>>> A.__slots__
()
>>> B.__slots__
()
>>> C.__slots__
()
```

We currently emit a false-positive on the `C.__slots__` attribute access above, and after https://github.com/astral-sh/ruff/pull/22327 we will also start emitting false-positive errors on the `A.__slots__` and `B.__slots__` attribute accesses too. We should fix this by synthesizing the generated `__slots__` attribute for these classes.

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-14 13:06_

---

_Comment by @charliermarsh on 2026-01-14 13:23_

I'm happy to take it as a follow-up.

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-14 13:23_

---
