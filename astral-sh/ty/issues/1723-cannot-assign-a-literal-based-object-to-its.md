```yaml
number: 1723
title: "Cannot assign a `literal`-based object to its corresponding `collections.abc`-based object"
type: issue
state: closed
author: mflova
labels: []
assignees: []
created_at: 2025-12-02T15:56:38Z
updated_at: 2025-12-02T15:59:26Z
url: https://github.com/astral-sh/ty/issues/1723
synced_at: 2026-01-10T01:58:59Z
```

# Cannot assign a `literal`-based object to its corresponding `collections.abc`-based object

---

_Issue opened by @mflova on 2025-12-02 15:56_

### Summary

I could not find a similar issue.

So far I found these problems with `dict/Mapping` and `list/Sequence` that seem to be very similar:

---

With `Mapping`-based:

```py
from collections.abc import Sequence
from typing import Literal, Mapping, TypedDict

def func(my_param: dict[Literal["a", "b"], int]) -> None:
    pass
func({"a": 1, "b": 2})  # OK

def func2(my_param: Mapping[Literal["a", "b"], int]) -> None:
    pass

func2({"a": 1, "b": 2})  #  error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `Mapping[Literal["a", "b"], int]`, found `dict[Unknown | str, Unknown | int]`
```

---

With `Sequence`-based:

```py
from collections.abc import Sequence
from typing import Literal, Mapping, TypedDict


class MyData(TypedDict):
    a: int
    b: int

def func(my_param: list[MyData]) -> None:
    pass

func([{"a": 1, "b": 2}, {"a": 3, "b": 4}])  # OK

def func2(my_param: Sequence[MyData]) -> None:
    pass

func2([{"a": 1, "b": 2}, {"a": 3, "b": 4}])  #  error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `Sequence[MyData]`, found `list[Unknown | dict[Unknown | str, Unknown | int]]`
```

### Version

ty 0.0.1-alpha.29

---

_Comment by @AlexWaygood on 2025-12-02 15:59_

Thanks for the report!

---

_Closed by @AlexWaygood on 2025-12-02 15:59_

---
