```yaml
number: 274
title: Improve diagnostics for failed call to overloaded function
type: issue
state: closed
author: carljm
labels:
  - diagnostics
  - calls
  - overloads
assignees: []
created_at: 2025-05-08T14:17:59Z
updated_at: 2025-05-15T15:46:06Z
url: https://github.com/astral-sh/ty/issues/274
synced_at: 2026-01-12T15:54:22Z
```

# Improve diagnostics for failed call to overloaded function

---

_@carljm_

Currently we emit a very opaque "No overload of bound method `__init__` matches" diagnostic, with zero additional context: no reference to the called function, its overloads, what failed to match, etc.

Sample code:

```py
from typing import overload

@overload
def f(x: int) -> int: ...

@overload
def f(x: str) -> str: ...

def f(x: int | str) -> int | str:
    return x

f(b"foo")
```

---

_Label `diagnostics` added by @carljm on 2025-05-08 14:18_

---

_Label `calls` added by @AlexWaygood on 2025-05-11 07:45_

---

_Label `overloads` added by @AlexWaygood on 2025-05-11 07:45_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-05-13 14:17_

---

_Comment by @sharkdp on 2025-05-14 19:10_

This has been closed in https://github.com/astral-sh/ruff/pull/18073

---

_Closed by @sharkdp on 2025-05-14 19:10_

---

_Comment by @dhruvmanila on 2025-05-15 00:00_

Re-opening this because this currently only works for `FunctionType`, it should be applied to other overloaded callables as well like:
* `BoundMethodType` - contains the `FunctionType`

  ```py
  import tempfile
  
  tempfile.TemporaryDirectory()
  ```

* `ClassLiteral` - this will be tricky because it could be either `__new__` or `__init__`

   ```py
   type()
   ```

Others are mentioned [here](https://github.com/astral-sh/ruff/blob/466021d5e1793ca30e0d860cb1cd27e32f5233aa/crates/ty_python_semantic/src/types/call/bind.rs#L1504-L1523) for which I'm not sure if it's worth the effort.

cc @BurntSushi, feel free to close it if you prefer to track this in a separate issue

---

_Reopened by @dhruvmanila on 2025-05-15 00:00_

---

_Closed by @BurntSushi on 2025-05-15 15:39_

---

_Comment by @BurntSushi on 2025-05-15 15:46_

I created #409 to track remaining work here. Thanks for the call out @dhruvmanila!

---
