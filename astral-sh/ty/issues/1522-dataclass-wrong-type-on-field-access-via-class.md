```yaml
number: 1522
title: "dataclass: wrong type on field access via class"
type: issue
state: open
author: Tinche
labels:
  - dataclasses
  - attribute access
assignees: []
created_at: 2025-11-11T14:55:02Z
updated_at: 2025-11-11T19:34:52Z
url: https://github.com/astral-sh/ty/issues/1522
synced_at: 2026-01-12T15:54:25Z
```

# dataclass: wrong type on field access via class

---

_@Tinche_

### Summary

MRE:

```python
from dataclasses import dataclass

@dataclass
class A:
    a: int

@dataclass(slots=True)
class ASlot:
    a: int

reveal_type(A.a)
reveal_type(ASlot.a)
```

Playground link: https://play.ty.dev/1eb91b94-d161-45d1-9e98-81630885079e

In both cases, ty reveals `int`. At runtime, the first case will raise an AttributeError (`a` does not exist on the class), and in the second the type is completely different [a member_descriptor instance that all slot classes have]).

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Label `dataclasses` added by @AlexWaygood on 2025-11-11 15:15_

---

_Label `attribute access` added by @AlexWaygood on 2025-11-11 15:15_

---

_Comment by @carljm on 2025-11-11 15:40_

Thanks for the report!

For reference, although it doesn't match runtime, our current behavior matches both [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&code=GYJw9gtgBAJghgFzgYwDZwM4YKYagSwgAcwQFZEV0sAoGgAXiTUwxpaygEEAuGqAVDg8CAOwR1GlDhgAUGVGAQYAvABUQAV2wBKdtTxcAyooR9BQkfnF0Q2AG7Y4qAPoIAnkWyyuAOjh6do7Obp7exqb%2BOkA) and [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEmqALqgMZSpxzx0Q3G5KTBszYcuAHXRSAAoxbtOcKYq50Agoil0ddVIl7omU2fLFKAFHCi4mcALwAVSgFcYAShXi4GgMo2mLXRdPQMIIxN0ShgANxhUKAB9JlJiGAt1QlRPKNj4pJS0jP9bLJyQABoQFyZoOBJyRBAAYjoAVVqoCBS6MBd0VlrcdGVpLBgwXsEaZkT0FxpsGEoLfDCjdzoAWgA%2BOjgmSiCQ6KYXSmCwCRAAOQWlo7pgfABfa6lKkDJosChSQiYtCgFFaAAVSD8-vsMDgCHRWMNIABzc7MCDDQhSVq%2BGAwOgACyYTGIcEQAHoyd8Jn9CIIkWSYOgyZhcKw4GSEehkaihkyppQ9DFUNBUNhYPDERAUZQ0cM6LhiLz6lIyEx8cMtnFKHB0cF7HRrgBmQgARgATO90CAXlU2LU4gAxaAwChoLB4Ihka1AA), and also matches the revealed types of [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=e7d1511134f2aa562af0ae8970c7feee). Mypy issues an additional error on `ASlot.a`, which is kind of weird since that's the one of the two above cases that _doesn't_ emit a runtime error.

If we were to try to model the runtime behavior more accurately, we also would need to consider the presence or absence of a default value. This makes no difference with `slots=True` (in that case all fields are always member descriptors on the class), but with `slots=False` any field with a default value returns the default value if accessed as an attribute on the class.

There are additional subtleties if descriptors are used as field types...

---

_Comment by @Tinche on 2025-11-11 16:00_

> If we were to try to model the runtime behavior more accurately, we also would need to consider the presence or absence of a default value. This makes no difference with slots=True (in that case all fields are always member descriptors on the class), but with slots=False any field with a default value returns the default value if accessed as an attribute on the class.

Huh, interesting. In attrs we chose to clean up the class vars to the best of our ability (for slotted classes obviously not possible); looks like dataclasses went another way. The idea being to always have to use `attrs.fields()` for this information.

And it looks like if the field doesn't have a default but a factory, dataclasses will not leave it on the class.

To continue on this tangent: my first thought was if the default wasn't provided dataclasses will let attribute access go to the class `__dict__`, but doesn't seem so - the default is also in the signature for the `__init__`.

---

_Comment by @carljm on 2025-11-11 16:14_

> In attrs we chose to clean up the class vars to the best of our ability (for slotted classes obviously not possible); looks like dataclasses went another way.

I think this is my main concern with spending effort modeling runtime behavior of dataclasses more closely. This behavior isn't specified or standardized for `dataclass_transform`, so it's likely that whatever we do will be wrong for some dataclass-transform-using libraries.

Which leaves me slightly more inclined to just continue to match the other type checkers -- if it's important to get this behavior right, then it would be worth standardizing it for `dataclass_transform`.

---

_Comment by @Tinche on 2025-11-11 19:11_

Understood, but the case when there is no default is pretty clear-cut, right?

The discussion about default handling can be left for fine tuning sometime in the future I suppose.

---

_Comment by @carljm on 2025-11-11 19:34_

> Understood, but the case when there is no default is pretty clear-cut, right?

I guess a dataclass-transform-using library could do something else there, but it's quite unlikely, so probably yes?

I think it's a low priority for ty to emit an error on these accesses, but in principle I think it would be fine.

---
