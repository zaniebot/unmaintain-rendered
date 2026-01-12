```yaml
number: 2198
title: "Invalid diagnostic location for a sub-call to a specialized `ParamSpec`"
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-12-24T07:17:27Z
updated_at: 2025-12-24T07:19:14Z
url: https://github.com/astral-sh/ty/issues/2198
synced_at: 2026-01-12T15:54:26Z
```

# Invalid diagnostic location for a sub-call to a specialized `ParamSpec`

---

_@dhruvmanila_

### Summary

Given the following example:
```py
from typing import Callable

def fn(a: int) -> None: ...

def foo[**P, T](fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs): ...

foo(fn, a="a")
```

The diagnostic is using the location for `fn` for highlighting on `foo`:

```console
❯ ty check .                                                          
error[invalid-argument-type]: Argument to function `foo` is incorrect
  --> main.py:10:5
   |
10 | foo(fn, a="a")
   |     ^^ Expected `int`, found `Literal["a"]`
   |
info: Function defined here
 --> main.py:7:5
  |
7 | def foo[**P, T](fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs): ...
  |     ^^^         ------------------ Parameter declared here
  |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

- The "Expected `...`, found `...`" should be on `"a"`
- It should use the `fn` function directly for the info diagnostic of "Function defined here"
- And, provide a sub-diagnostic / info to state that this function comes from the argument being matched against a `ParamSpec` type variable which resolved to this function

### Version

_No response_

---

_Label `diagnostics` added by @dhruvmanila on 2025-12-24 07:17_

---

_Label `bug` added by @dhruvmanila on 2025-12-24 07:17_

---

_Renamed from "Improve diagnostic for a sub-call to a specialized `ParamSpec`" to "Invalid diagnostic location for a sub-call to a specialized `ParamSpec`" by @dhruvmanila on 2025-12-24 07:17_

---

_Added to milestone `Stable` by @dhruvmanila on 2025-12-24 07:18_

---
