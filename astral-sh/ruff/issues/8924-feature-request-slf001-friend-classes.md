```yaml
number: 8924
title: "[feature request] SLF001. Friend classes."
type: issue
state: closed
author: kshpytsya
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-11-30T13:29:01Z
updated_at: 2023-12-07T03:12:40Z
url: https://github.com/astral-sh/ruff/issues/8924
synced_at: 2026-01-12T15:54:48Z
```

# [feature request] SLF001. Friend classes.

---

_@kshpytsya_

I do realize that ruff's `SLF001` is modeled after `flake8-self` and a such, there may be opposition for implementing behavior diverging from the latter.
However, I often find the situation that boils down to the following:

```sh
$ cat a.py
```
```py
class Accessor:
    def __init__(self, parent: "Parent") -> None:
        self._parent = parent

    def act(self) -> None:
        self._parent._private()


class Parent:
    def _private(self) -> None:
        pass

    def get_accessor(self) -> Accessor:
        return Accessor(self)
```
```sh
$ ruff check --select SLF001 a.py
a.py:6:9: SLF001 Private member accessed: `_private`
Found 1 error.
```

In actual code `Accessor` contains multiple references to `Parent`'s private members. Currently, to avoid `SLF001`, every such line should be annotated with `# noqa: SLF001`. I would not want to disable `SLF001` globally as I believe it to be useful, however, it would be great to have something similar to C++'s [friend class](https://www.geeksforgeeks.org/friend-class-function-cpp/) which would allow one to mark `Accessor` as a friend of `Parent`.

---

_Label `rule` added by @dhruvmanila on 2023-11-30 21:34_

---

_Label `needs-decision` added by @dhruvmanila on 2023-11-30 21:34_

---

_Comment by @charliermarsh on 2023-12-07 03:12_

Ahh yeah, I think this is gonna be hard to support, since we don't have a builtin syntax or `typing` mechanic to express it in code, and even then, we'd need to be able to do stronger type inference than Ruff is capable of today.

---

_Closed by @charliermarsh on 2023-12-07 03:12_

---
