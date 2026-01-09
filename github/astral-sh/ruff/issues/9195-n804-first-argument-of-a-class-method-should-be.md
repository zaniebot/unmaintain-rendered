---
number: 9195
title: "\"N804 First argument of a class method should be named `cls`\" on a property of a metaclass"
type: issue
state: closed
author: ZeeD
labels:
  - bug
assignees: []
created_at: 2023-12-19T09:13:53Z
updated_at: 2023-12-22T08:40:38Z
url: https://github.com/astral-sh/ruff/issues/9195
synced_at: 2026-01-07T13:12:15-06:00
---

# "N804 First argument of a class method should be named `cls`" on a property of a metaclass

---

_Issue opened by @ZeeD on 2023-12-19 09:13_

minimal case:

```python
class C:
    @property
    def foo(self) -> str:
        return 'foo'


class M(type):
    @property
    def foo(self) -> str:
        return 'foo'
```

`ruff` (current version, `0.1.8`) says that, while `C.foo` first arguments should be called `self`, for `M` (that is a metaclass) the `foo` method first argument should be called `cls` - as it is a class method


```
src/metaproperty.py:9:13: N804 First argument of a class method should be named `cls`
```

(honestly, I'm not even sure it's a bug or a feature, as I am trying to achieve something like a `@staticproperty`, but that's not really relevant here)

---

_Label `bug` added by @charliermarsh on 2023-12-19 21:10_

---

_Comment by @charliermarsh on 2023-12-19 21:11_

I think you're correct -- we should change this rule when within a metaclass. Thanks!

---

_Comment by @picnixz on 2023-12-20 08:32_

I would also like to point out that the `__new__` method in a metaclass should allow `mcs` or `mcls` as its first argument and not enforce `cls`. More generally, I would like to suggest a way to specify which names are allowed as the first argument for a method or a classmethod on a metaclass (by default it should be `cls` and `mcs/mcls` respectively but I know that some codes still use `__mcs` or `__mcls` if they don't support the positional-only arguments).

---

_Comment by @charliermarsh on 2023-12-20 17:05_

Actually, I think this might be correct. I thought we were suggesting `self` but we're suggesting `cls` as you said above, which seems right to me...


---

_Closed by @charliermarsh on 2023-12-20 17:05_

---

_Comment by @ZeeD on 2023-12-22 06:51_

Hi @charliermarsh 

Thinking about it again...
I'm not sure it is correct: `foo` is a *normal* method, that just appear to be defined on a class that extends `type`.
When you access the property value (for example with a `bar = M().foo`) the first parameter is an instance of `M`, not the `M` class itself, even when `M` is actually used as metaclass
I'm failing to grasp the reason behind expecting the first parameter to be called `cls`.

In https://github.com/astral-sh/ruff/issues/9195#issuecomment-1864062666 moreover, there is a reference to `__new__`, but it's just half relevant here: `__new__` is a *static* method, so it should have `cls` by definition (as it has commonly). Yet I understand, for `__new__` specifically, to suggest the usage of `mcls` for metaclasses, as, generally, `cls` is used to the variable that should be returned. But for this case I would suggest to open a dedicated issue, as it is not really relevant here for me.



---

_Comment by @picnixz on 2023-12-22 08:40_

Actually, I commented because it fell under N804 but I should have probably made a separate issue. 


---
