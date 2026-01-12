```yaml
number: 947
title: "Incorrect types inferred when a \"mixed tuple\" is unpacked"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2025-08-06T16:36:33Z
updated_at: 2025-08-22T13:11:21Z
url: https://github.com/astral-sh/ty/issues/947
synced_at: 2026-01-12T15:54:24Z
```

# Incorrect types inferred when a "mixed tuple" is unpacked

---

_@AlexWaygood_

### Summary

Consider:

```py
class I0: ...
class I1: ...
class I2: ...

def f(x: tuple[I0, *tuple[I1, ...], I2]):
    [a, b, *c] = x
    reveal_type(a)  # I0
    reveal_type(b)  # I1
    reveal_type(c)  # list[I1 | I2]
```

The types we infer for `a` and `c` seem correct here. But the type for `b` seems incorrect. There are three possible scenarios in which this assignment could succeed; I think we need to consider them all and union the types together:
1. The middle element "materialises" to a 0-length tuple. In this scenario at runtime, `a` would be of type `I0`, `b` would be of type `I2` and `c` would be of type `list[Never]`
2. The middle element "materialises" to a tuple of length 1. In this scenario at runtime, `a` would be of type `I0`, `b` would be of type `I1` and `c` would be of type `list[I2]`
3. The middle element "materialises" to a tuple of length >=2. In this scenario at runtime, `a` would be of type `I0`, `b` would be of type `I1` and `c` would be of type `list[I1 | I2]`

Given these three scenarios in which the assignment could succeed, I think we should be inferring `I1 | I2` for `b` rather than `I1`.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-08-06 16:36_

---

_Comment by @AlexWaygood on 2025-08-06 16:39_

Similarly:

```py
class I0: ...
class I1: ...
class I2: ...

def f(x: tuple[I0, *tuple[I1, ...], I2]):
    [a, b, c, *d] = x
    reveal_type(a)  # I0
    reveal_type(b)  # I1
    reveal_type(c)  # I1
    reveal_type(d)  # list[I1 | I2]
```

The type of `c` here should also be `I1 | I2` rather than just `I1`, I think

---

_Comment by @carljm on 2025-08-06 17:24_

For reference, other existing type checkers all do arguably worse here than our current behavior; for the first example pyright infers `I0, I1, list[I2]` (so missing the possibility that `b` could be `I2`, and also the possibility that there could be an `I1` in `c`), mypy throws a (clearly wrong?) unpack error and then infers a weird `Unpack` type for `b`, and pyrefly doesn't attempt any unpack logic at all, it just infers `I0 | I1 | I2` for `a` and `b` and `list[I0 | I1 | I2]` for `c` (at least this version is not _wrong_, even if it lacks precision.)

---

_Comment by @jelle-openai on 2025-08-06 18:10_

Tuples containing mixed variable-length and fixed-length components can be a rabbit hole for lots of complicated logic that almost nobody is going to care about (when does one use a type like `tuple[A, *tuple[B, ...], C]` in practice?). 

> other existing type checkers all do arguably worse here

```
% python -m pycroscope -c '''
class I0: ...
class I1: ...
class I2: ...

def f(x: tuple[I0, *tuple[I1, ...], I2]):
    [a, b, *c] = x
    reveal_type(a)  # I0
    reveal_type(b)  # I1
    reveal_type(c)  # list[I1 | I2]
''' --output-format concise
<code>:8:16: Revealed type is 'I0' [reveal_type]
<code>:9:16: Revealed type is 'I1 | I2' [reveal_type]
<code>:10:16: Revealed type is 'list[I1 | I2]' [reveal_type]
```



---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:11_

---
