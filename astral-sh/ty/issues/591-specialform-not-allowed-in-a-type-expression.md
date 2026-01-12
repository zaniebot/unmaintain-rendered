```yaml
number: 591
title: "`_SpecialForm` not allowed in a type expression"
type: issue
state: closed
author: loicdiridollou
labels: []
assignees: []
created_at: 2025-06-06T01:25:05Z
updated_at: 2025-11-12T02:01:41Z
url: https://github.com/astral-sh/ty/issues/591
synced_at: 2026-01-12T15:54:23Z
```

# `_SpecialForm` not allowed in a type expression

---

_@loicdiridollou_

### Summary

Hey ty team,
We are trying to expand the use of `ty` on `pandas-stubs` and seeing any weird behaviors.
I came across the following one where we pass a union of types in a list:

```python
from typing import (
    Callable,
    Hashable,
    Mapping,
    TypeAlias,
)

AggFuncTypeBase: TypeAlias = Callable | str
AggFuncTypeDictSeries: TypeAlias = Mapping[Hashable, AggFuncTypeBase]
```
The last line would raise the following error:
`error[invalid-type-form]: Variable of type `_SpecialForm` is not allowed in a type expression`

We expect that behavior to pass fine, one interesting fix is that if you replace the `AggFuncTypeBase` union syntax with the old `Union[...]` syntax, that passes fine.
Sorry for the weird naming of variables I used the bug we found in `pandas-stubs` directly.

The whole example is in the playground https://play.ty.dev/18ee0638-3d8d-41eb-919d-45f17f5302e1

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @carljm on 2025-06-06 01:41_

Thanks for the report. Yes, this is a known issue that we don't support `typing.TypeAlias` yet. Currently support for that is held up by our work on supporting recursive type aliases, since `typing.TypeAlias` is used for some recursive aliases in typeshed.

---

_Closed by @carljm on 2025-06-06 01:41_

---

_Comment by @carljm on 2025-11-12 02:01_

(This is now fixed)

---
