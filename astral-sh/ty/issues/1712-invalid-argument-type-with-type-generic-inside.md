```yaml
number: 1712
title: "`invalid-argument-type` with `type[]` generic inside union "
type: issue
state: closed
author: aidandj
labels:
  - generics
assignees: []
created_at: 2025-12-01T23:17:11Z
updated_at: 2025-12-02T23:25:10Z
url: https://github.com/astral-sh/ty/issues/1712
synced_at: 2026-01-10T01:56:40Z
```

# `invalid-argument-type` with `type[]` generic inside union 

---

_Issue opened by @aidandj on 2025-12-01 23:17_

Starting in alpha 29 this code started erroring. 


https://play.ty.dev/45e470fd-853e-4d61-9cfa-655806adc789

```py
from typing import Union, Dict, Type, TypeGuard, TypeVar

class Base: ...

T = TypeVar("T", bound=Base)


def is_class_dict(
    val: Union[Dict[Type[T], str], Dict[str, str]],
):
    for k in val.keys(): # Argument to bound method `keys` is incorrect: Argument type `dict[type[T@is_class_dict], str]` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
        pass
```

_Originally posted by @aidandj in https://github.com/astral-sh/ty/issues/1685#issuecomment-3592196876_

---

_Added to milestone `Beta` by @carljm on 2025-12-02 01:09_

---

_Label `generics` added by @carljm on 2025-12-02 01:09_

---

_Comment by @sharkdp on 2025-12-02 07:53_

Thank you for reporting this.

I further minified this to
```py
def f[T](val: dict[type[T], str]):
    val.keys()
```
https://play.ty.dev/7e20a0a6-a6a0-4296-832c-4792bc5be2bd

We apparently fail to see that `type[T@f]` is assignable to the (inferable) `_KT@dict`.

FYI @ibraheemdev

---

_Comment by @aidandj on 2025-12-02 17:26_

Similar place in my code, wondering if this is related or separate. Assuming its caused by the failure to assign to the dict type again. Can file another issue if separate:

https://play.ty.dev/5a17c0d8-10a9-41a4-9c46-0206c3904935

```
from typing import Union, Dict, Type, TypeGuard, TypeVar

class Base: ...

T = TypeVar("T", bound=Base)

def iterate(d: Union[Dict[str, int] | Dict[Type[T], int]]):
    for v in d: # Object of type `dict[str, int] | dict[type[T@iterate], int]` may not be iterable (not-iterable) [Ln 8, Col 14]
        print(d)
```

And similarly minified to yours

https://play.ty.dev/cbe7c333-eb94-47dc-8809-31808956eb45

```
def f[T](val: dict[type[T], str]):
    for v in val: # Object of type `dict[type[T@f], str]` is not iterable (not-iterable) [Ln 2, Col 14]
        print(v)
```

---

_Closed by @ibraheemdev on 2025-12-02 23:25_

---
