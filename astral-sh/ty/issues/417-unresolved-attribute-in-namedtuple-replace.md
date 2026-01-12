```yaml
number: 417
title: "[unresolved-attribute] in NamedTuple `_replace`"
type: issue
state: closed
author: varchasgopalaswamy
labels:
  - bug
  - runtime semantics
  - namedtuples
assignees: []
created_at: 2025-05-16T05:49:39Z
updated_at: 2025-11-29T18:36:28Z
url: https://github.com/astral-sh/ty/issues/417
synced_at: 2026-01-12T15:54:23Z
```

# [unresolved-attribute] in NamedTuple `_replace`

---

_@varchasgopalaswamy_

### Summary

Here's a simple reproducer for this issue 

```python

class TestTuple(NamedTuple):
    a: int
    b: str
    c: float
    d: list[int]


test = TestTuple(a=1, b="test", c=3.14, d=[1, 2, 3])
updated = test._replace(a=2, b="updated")
print(updated)
```

Running the script works as expected
```bash
> python3 test.py
TestTuple(a=2, b='updated', c=3.14, d=[1, 2, 3])
```

But running `ty check` fails 
```bash 
> ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `TestTuple` has no attribute `_replace`
  --> test.py:18:11
   |
17 | test = TestTuple(a=1, b="test", c=3.14, d=[1, 2, 3])
18 | updated = test._replace(a=2, b="updated")
   |           ^^^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default
```

### Version

`ty 0.0.1-alpha.3 (144a26d44 2025-05-15)`

---

_Label `bug` added by @sharkdp on 2025-05-16 05:59_

---

_Comment by @sharkdp on 2025-05-16 06:02_

Thank you for reporting this.

When looking up attributes on `NamedTuple` instances, we should fall back to attributes on [`_typeshed._type_checker_internals.NamedTupleFallback`](https://github.com/python/typeshed/blob/126768408a69b7a3a09b7d3992970b289f92937e/stdlib/_typeshed/_type_checker_internals.pyi#L54-L75).

---

_Label `runtime semantics` added by @sharkdp on 2025-05-16 06:02_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-16 06:35_

---

_Closed by @sharkdp on 2025-05-16 10:56_

---

_Closed by @sharkdp on 2025-05-16 10:56_

---

_Label `namedtuples` added by @AlexWaygood on 2025-11-29 18:36_

---
