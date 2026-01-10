```yaml
number: 1534
title: "support `type[...]` types in implicit type aliases"
type: issue
state: closed
author: loicdiridollou
labels:
  - bug
assignees: []
created_at: 2025-11-12T22:50:24Z
updated_at: 2025-11-13T07:21:21Z
url: https://github.com/astral-sh/ty/issues/1534
synced_at: 2026-01-10T02:06:25Z
```

# support `type[...]` types in implicit type aliases

---

_Issue opened by @loicdiridollou on 2025-11-12 22:50_

### Summary

With the last release we have seen some breaking behavior in `ty` (1a26)
Playground is https://play.ty.dev/fae7deba-76eb-45bf-87a3-02871d95e3c3

The error we are getting is:
`error[invalid-type-form]: Variable of type UnionType is not allowed in a type expression` 
which we did not get with version 1a25.

Not sure what caused this change but curious to know how it appeared under the hood.
For reference the code from the playground is below:

```python
from collections.abc import (
    Hashable,
    Mapping,
)
from typing import TypeAlias

# dtypes
NpDtypeNoStr: TypeAlias = type[str] | type[complex | bool | object]
NpDtype: TypeAlias = str | NpDtypeNoStr | type[str]
Dtype: TypeAlias = NpDtypeNoStr | NpDtype
# DtypeArg specifies all allowable dtypes in a functions its dtype argument
DtypeArg: TypeAlias = Dtype | Mapping[Hashable, Dtype]
```

### Version

ty 0.0.1a26

---

_Label `bug` added by @carljm on 2025-11-12 22:59_

---

_Comment by @carljm on 2025-11-12 23:00_

Thanks for the report! Sorry, this is indeed a bug in our handling of type aliases.

@sharkdp it looks like we aren't handling `type[...]` (`SubclassOf`) types in unions in type aliases.

---

_Renamed from "Issue with UnionType not allowed in type expression" to "support `type[...]` types in implicit type aliases" by @carljm on 2025-11-12 23:34_

---

_Added to milestone `Beta` by @carljm on 2025-11-13 06:57_

---

_Comment by @sharkdp on 2025-11-13 07:21_

> [@sharkdp](https://github.com/sharkdp) it looks like we aren't handling `type[...]` (`SubclassOf`) types in unions in type aliases.

Correct. It's next on my list here: https://github.com/astral-sh/ty/issues/221#issuecomment-3482599572

---

_Closed by @sharkdp on 2025-11-13 07:21_

---
