```yaml
number: 20808
title: lint .value from StrEnum
type: issue
state: open
author: asukaminato0721
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-11T08:09:28Z
updated_at: 2025-10-14T17:47:01Z
url: https://github.com/astral-sh/ruff/issues/20808
synced_at: 2026-01-12T15:54:57Z
```

# lint .value from StrEnum

---

_@asukaminato0721_

### Summary

```py
class A(StrEnum):
    AA = auto()
    BB = auto()

A.AA.value # this trigger a lint
A.AA # this is good
```


---

_Comment by @MichaReiser on 2025-10-12 06:44_

@amyreese what do you think of such a rule?

---

_Label `rule` added by @MichaReiser on 2025-10-12 06:44_

---

_Label `needs-decision` added by @MichaReiser on 2025-10-12 06:44_

---

_Comment by @sharkdp on 2025-10-13 07:52_

@asukaminato0721 Can you clarify why you would want `A.AA.value` to be flagged? As far as I can tell, there is nothing wrong with accessing `.value` on `StrEnum` (it's equivalent to `"aa"` for this example using `auto()`).

Do you want to avoid confusion between the actual enum member (`A.AA`) and that string value (`A.AA.value == "aa"`)? I think there are valid use cases for accessing `.value` on an enum member, so it's unclear to me how you would avoid false positives.

---

_Comment by @asukaminato0721 on 2025-10-13 08:48_

I am migrating from `Enum` to `StrEnum`, then I find use `A.AA` is shorter, and users don't need to know the `.value` behind it. So I think unifying them is a method.


---

_Comment by @amyreese on 2025-10-14 17:44_

I think this could be a reasonable rule. With `StrEnum` specifically, the enum element itself is intended to be used in place of `.value`, and has all/most of the properties of a normal string:

```pycon
>>> from enum import StrEnum, auto
>>> class Foo(StrEnum):
...     alpha = auto()
...     beta = auto()
...     gamma = auto()
...     delta = auto()
...
>>> Foo.alpha
<Foo.alpha: 'alpha'>
>>> str(Foo.alpha)
'alpha'
>>> Foo.alpha == "alpha"
True
>>> Foo.alpha.startswith("a")
True
>>> sorted(Foo)
[<Foo.alpha: 'alpha'>, <Foo.beta: 'beta'>, <Foo.delta: 'delta'>, <Foo.gamma: 'gamma'>]
```

---
