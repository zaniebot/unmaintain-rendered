---
number: 18442
title: "F841 and PLE0117 don’t understand `__class__` bindings"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-06-03T15:07:44Z
updated_at: 2025-08-26T13:49:10Z
url: https://github.com/astral-sh/ruff/issues/18442
synced_at: 2026-01-07T13:12:16-06:00
---

# F841 and PLE0117 don’t understand `__class__` bindings

---

_Issue opened by @dscorbett on 2025-06-03 15:07_

### Summary

By default, `__class__` within a method definition is a free variable and its corresponding cell variable is always bound. [`unused-variable` (F841)](https://docs.astral.sh/ruff/rules/unused-variable/) and [`nonlocal-without-binding` (PLE0117)](https://docs.astral.sh/ruff/rules/nonlocal-without-binding/) have false positives because they don’t understand this special case. This is probably a general problem with how Ruff resolves bindings which could affect other rules.

```console
$ cat >f841_ple0117.py <<'# EOF'
class A:
    x = 1


class B:
    x = 2


class C(A, B):
    def set_class(self, cls):
        nonlocal __class__
        if cls not in type(self).__mro__:
            raise ValueError(f"Invalid superclass: {cls}")
        __class__ = cls

    def get_class(self):
        return __class__

    def __getattribute__(self, attr):
        if attr in vars(type(self)):
            return object.__getattribute__(self, attr)
        if attr in vars(__class__):
            mro = type(self).__mro__
            return getattr(super(mro[mro.index(__class__) - 1], self), attr)
        return getattr(super(), attr)


c = C()
print(c.x)
c.set_class(B)
print(c.x)
# EOF

$ python f841_ple0117.py
1
2

$ ruff --isolated check --select F841,PLE0117 f841_ple0117.py --output-format concise -q
f841_ple0117.py:11:18: PLE0117 Nonlocal name `__class__` found without binding
f841_ple0117.py:14:9: F841 Local variable `__class__` is assigned to but never used

$ ruff --isolated check --select F841,PLE0117 f841_ple0117.py -s --unsafe-fixes --fix

$ python f841_ple0117.py
1
1
```

That PLE0117 diagnostic is a false positive because `__class__` does have a binding; it just isn’t explicit in the source code. That F841 diagnostic is a false positive because `__class__` is not a local variable.

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Label `bug` added by @ntBre on 2025-06-03 15:23_

---

_Comment by @ntBre on 2025-06-03 15:28_

Wow, another interesting one! I assumed that `__class__` would also be bound in the class definition itself, but this is not the case:

```pycon
>>> class C:
...     print(__class__)
...
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    class C:
        print(__class__)
  File "<python-input-6>", line 2, in C
    print(__class__)
          ^^^^^^^^^
NameError: name '__class__' is not defined
```

Maybe we can add a synthetic binding for methods or something like that.

Nesting also works:

```pycon
>>> class A:
...     def foo():
...         class B:
...             print(__class__)
...
>>> A.foo()
<class '__main__.A'>
```

---

_Comment by @mikeleppane on 2025-08-06 11:04_

I am currently working on this.

---

_Assigned to @mikeleppane by @ntBre on 2025-08-06 11:57_

---

_Referenced in [astral-sh/ruff#19783](../../astral-sh/ruff/pulls/19783.md) on 2025-08-06 15:41_

---

_Referenced in [astral-sh/ruff#20048](../../astral-sh/ruff/pulls/20048.md) on 2025-08-22 17:42_

---

_Closed by @ntBre on 2025-08-26 13:49_

---
