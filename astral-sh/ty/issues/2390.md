---
number: 2390
title: "feat: better diagnostics when instance does not satisfy `abc.collections.<Class>`"
type: issue
state: closed
author: NickCrews
labels: []
assignees: []
created_at: 2026-01-08T03:53:52Z
updated_at: 2026-01-08T13:27:03Z
url: https://github.com/astral-sh/ty/issues/2390
synced_at: 2026-01-10T01:51:14Z
---

# feat: better diagnostics when instance does not satisfy `abc.collections.<Class>`

---

_Issue opened by @NickCrews on 2026-01-08 03:53_

I am working in the ibis project, trying to fix up its typing so that [it's `Schema` class](https://github.com/ibis-project/ibis/blob/e5921c9a851afc8fd3980f6a0bbfd73720b4d5d6/ibis/expr/schema.py#L25) typechecks against a `collections.abc.Mapping[str, ibis.DataType]`. It currently does not.

You can reproduce this with `uv pip install ibis-framework` and then

```python
from collections.abc import Mapping

import ibis

s: Mapping[str, ibis.DataType] = ibis.schema({"a": "int64", "b": "string"})
```

This gives the type error ``Object of type `Schema` is not assignable to `Mapping[str, DataType]`: Incompatible value of type `Schema`ty[invalid-assignment](https://ty.dev/rules#invalid-assignment)``

It would be great if this error message instead told me why it isn't assignable. What required methods does it not implement? Is it that the methods have the wrong signatures? Or gave some more actionable advice, like I need to make ibis.Schema inherit from collections.abc.Mapping or call `collections.abc.Mapping.register(ibis.Schema)`

Thank you!

---

_Comment by @AlexWaygood on 2026-01-08 13:27_

Thanks!

---

_Closed by @AlexWaygood on 2026-01-08 13:27_

---
