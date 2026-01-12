```yaml
number: 2301
title: "Unexpected inferred type when using `typing.Literal`"
type: issue
state: closed
author: jeertmans
labels:
  - question
  - narrowing
  - bidirectional inference
assignees: []
created_at: 2026-01-02T10:31:15Z
updated_at: 2026-01-05T08:29:52Z
url: https://github.com/astral-sh/ty/issues/2301
synced_at: 2026-01-12T15:54:26Z
```

# Unexpected inferred type when using `typing.Literal`

---

_@jeertmans_

### Summary

Hi!

I recently faced an issue when trying to correctly annotate the following pattern:

```python
from typing import Literal, get_args

Value = Literal["a", "b"]
ALLOWED_VALUES = get_args(Value)

def f(x: str) -> Value:
    if x not in ALLOWED_VALUES:
        raise ValueError
    return x
```
where `ty` would complain that `foo` returns a `str` and not a `Value`.

I came up with different ideas to try to solve this issue, but none work, except for the last case, which is quite surprising. As you can see in the code below, [link to playground](https://play.ty.dev/c0941551-ec5a-4d3e-b06e-15b185d7ca46), the inferred type for `ALLOWED_VALUES_BAR` changes whether we are inside or outside the `bar` function.

<details>

<summary>Full Python code</summary>

```python
from typing import Any, Literal, assert_type, get_args

Value = Literal["a", "b"]

# Ty does not infer that x: tuple[Value, ...]
# probably because get_args is typed tuple[Any, ...]?
ALLOWED_VALUES_FOO = get_args(Value)

def foo(x: str) -> Value:
    if x not in ALLOWED_VALUES_FOO:
        raise ValueError
    reveal_type(x)
    return x

# Type hint...
ALLOWED_VALUES_BAR: tuple[Value, ...] = get_args(Value)
# ... seems to be ignored as it reveals tuple[Any, ...]
reveal_type(ALLOWED_VALUES_BAR)

def bar(x: str) -> Value:
    # but here it is revealed to be tuple[Literal["a", "b"], ...]
    reveal_type(ALLOWED_VALUES_BAR)
    if x not in ALLOWED_VALUES_BAR:
        raise ValueError
    reveal_type(x)
    return x

# Another way I tried to overcome this issue,
# but this requires duplicating the allowed literals
ALLOWED_VALUES_BAZ: tuple[Value, ...] = ("a", "b")

def baz(x: str) -> Value:
    if x not in ALLOWED_VALUES_BAZ:
        raise ValueError
    reveal_type(x)
    return x

# For some reasons, using x: Any works...

def baz_any(x: Any) -> Value:
    if x not in ALLOWED_VALUES_BAZ:
        raise ValueError
    reveal_type(x)
    return x
```

</details>

<details>

<summary>File diagnostic</summary>

```
Revealed type: `str` (revealed-type) [Ln 12, Col 17]
Return type does not match returned value: expected `Literal["a", "b"]`, found `str` (invalid-return-type) [Ln 13, Col 12]
Revealed type: `tuple[Any, ...]` (revealed-type) [Ln 18, Col 13]
Revealed type: `tuple[Literal["a", "b"], ...]` (revealed-type) [Ln 22, Col 17]
Revealed type: `str` (revealed-type) [Ln 25, Col 17]
Return type does not match returned value: expected `Literal["a", "b"]`, found `str` (invalid-return-type) [Ln 26, Col 12]
Revealed type: `str` (revealed-type) [Ln 35, Col 17]
Return type does not match returned value: expected `Literal["a", "b"]`, found `str` (invalid-return-type) [Ln 36, Col 12]
Revealed type: `Any` (revealed-type) [Ln 43, Col 17]
```

</details>

I hope this is not a duplicate of any existing issue.

Thanks in advance!


### Version

ty 0.0.8

---

_Comment by @mtshiba on 2026-01-02 22:11_

I think there are two main factors at play.

One is https://github.com/astral-sh/ty/issues/136. The other is that `str` is an inheritable class, which disables narrowing in tuples.

```python
from typing import Literal

Value = Literal["a", "b"]

class BadStr(str):
    def __eq__(self, other):
        return True

def f(x: str) -> Value:
    if x not in ("a", "b"):
        raise ValueError
    return x

x = f(BadStr("c"))
print(x)  # c
```

So you should probably use `LiteralString`.

```python
from typing import Literal, LiteralString

Value = Literal["a", "b"]

def f(x: LiteralString) -> Value:
    if x not in ("a", "b"):
        raise ValueError
    return x  # OK

def g(x: LiteralString) -> Value:
    if x not in ("a", "c"):
        raise ValueError
    return x # Not OK

```

---

_Label `narrowing` added by @mtshiba on 2026-01-02 22:11_

---

_Label `bidirectional inference` added by @mtshiba on 2026-01-02 22:11_

---

_Label `question` added by @MichaReiser on 2026-01-03 17:33_

---

_Comment by @carljm on 2026-01-04 21:08_

The issue for tracking the possibility that we could add some unsafe narrowing even when a subclass could override `__eq__` is https://github.com/astral-sh/ty/issues/1566

Will close this as duplicate - thanks for the report!

---

_Closed by @carljm on 2026-01-04 21:08_

---

_Comment by @jeertmans on 2026-01-05 08:29_

Thanks for your help and reply @mtshiba & @carljm!

---
