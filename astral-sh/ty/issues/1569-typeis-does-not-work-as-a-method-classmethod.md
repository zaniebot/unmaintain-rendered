```yaml
number: 1569
title: TypeIs does not work as a method/classmethod
type: issue
state: closed
author: carljm
labels:
  - bug
  - narrowing
  - typing semantics
assignees: []
created_at: 2025-11-14T23:31:27Z
updated_at: 2025-12-30T13:30:41Z
url: https://github.com/astral-sh/ty/issues/1569
synced_at: 2026-01-12T15:54:25Z
```

# TypeIs does not work as a method/classmethod

---

_@carljm_

In [this snippet](https://play.ty.dev/2f2334a7-0510-4d2c-8280-3c1f69983d18) all `reveal_type` should reveal `int`; currently all but the first reveal `object`.

```py
from typing import TypeIs

def is_int(val: object) -> TypeIs[int]:
  return isinstance(val, int)

class A:
  def is_int(self, val: object) -> TypeIs[int]:
    return isinstance(val, int)

  @classmethod
  def is_int2(cls, val: object) -> TypeIs[int]:
    return isinstance(val, int)

def _(x: object):
  if is_int(x):
    reveal_type(x)

  if A().is_int(x):
    reveal_type(x)

  if A().is_int2(x):
    reveal_type(x)

  if A.is_int2(x):
    reveal_type(x)
```

---

_Added to milestone `Stable` by @carljm on 2025-11-14 23:31_

---

_Label `bug` added by @carljm on 2025-11-14 23:31_

---

_Label `narrowing` added by @carljm on 2025-11-14 23:31_

---

_Label `typing semantics` added by @carljm on 2025-11-14 23:31_

---

_Comment by @NickCrews on 2025-12-22 16:19_

Hi! We have a similar need to [in Ibis](https://github.com/ibis-project/ibis/blob/b2e3100d9277a93fc1d8d97fb3f68856b9e6b91d/ibis/expr/datatypes/core.py#L365-L379), except we want the TypeIs to bind to the `self`, with no arguments, eg

```python

# The base class
class DataType:
    # ...lots of stuff..

    # Now have helpful checker methods to test for if this instance is a specific subclass
	def is_array(self) -> TypeIs[Array]:
        return isinstance(self, Array)

    def is_binary(self) -> TypeIs[Binary]:
        return isinstance(self, Binary)

    def is_boolean(self) -> TypeIs[Boolean]:
        return isinstance(self, Boolean)

    # etc
```

I'm not sure if this use case is supported in the PEP though, perhaps we would need to modify the PEP to explicitly mention this use case?

---

_Comment by @carljm on 2025-12-22 16:27_

@NickCrews Unfortunately I think that usage is not supported in the PEP, typing spec, or by other type checkers, so that would need discussion on discuss.python.org to reach consensus of type checkers to support this use case and an amendment to the typing spec.

---

_Comment by @erictraut on 2025-12-22 16:50_

@NickCrews, this use case was discussed during the drafting of PEP 647. You can read about it in the [rejected ideas section](https://peps.python.org/pep-0647/#narrowing-of-implicit-self-and-cls-parameters).

---

_Comment by @NickCrews on 2025-12-22 17:01_

Thank you both, sounds good, ibis will need to figure something out more standards-centric, sounds good that there is nothing for ty to do here.

---

_Comment by @MichaReiser on 2025-12-30 11:12_

@carljm the example in this issue now reveals `int` everywhere. Is there something more we want to do beyond the `typing.TypeGuard` PR or can we close it?

---

_Comment by @AlexWaygood on 2025-12-30 13:30_

Yes, this was fixed in https://github.com/astral-sh/ruff/pull/20974

---

_Closed by @AlexWaygood on 2025-12-30 13:30_

---
