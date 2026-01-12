```yaml
number: 485
title: "Incorrect `unsupported-operator` with operator `in` on types `str` and `T & str`"
type: issue
state: closed
author: shilch
labels:
  - bug
  - help wanted
  - typing semantics
assignees: []
created_at: 2025-05-22T08:30:24Z
updated_at: 2025-05-23T10:55:18Z
url: https://github.com/astral-sh/ty/issues/485
synced_at: 2026-01-12T15:54:23Z
```

# Incorrect `unsupported-operator` with operator `in` on types `str` and `T & str`

---

_@shilch_

### Summary

Minimal sample:

```python
def do_something[T](val: T, s: str):
    if type(val) is str:
        if s in val:
            print('s in val')
```

Error by ty:

```
error[unsupported-operator]: Operator `in` is not supported for types `str` and `T`, in comparing `str` with `T & str`
 --> test.py:3:12
  |
1 | def do_something[T](val: T, s: str):
2 |     if type(val) is str:
3 |         if s in val:
  |            ^^^^^^^^
4 |             print('s in val')
  |
info: rule `unsupported-operator` is enabled by default
```


### Version

ty 0.0.1-alpha.6

---

_Comment by @sharkdp on 2025-05-22 13:14_

Thank you for reporting this.

It seems like we mistakenly emit an `unsupported-operator` diagnostic if *any* of the members in a the intersection do not support the operator. But we should only report that if *none* of the members support it, I think.

More self-contained example:
```py
class Container:
    def __contains__(self, x) -> bool:
        return True

class NonContainer:
    pass

def f(x: Container):
    if isinstance(x, NonContainer):
        reveal_type(x)  # Container & NonContainer

        "a" in x  # should not be an error
```
https://play.ty.dev/8d1dabca-2623-4431-b08c-963d9e48ebb2

---

_Label `bug` added by @sharkdp on 2025-05-22 13:15_

---

_Label `typing semantics` added by @sharkdp on 2025-05-22 13:15_

---

_Comment by @sharkdp on 2025-05-22 13:19_

This needs to be fixed in `TypeInferenceBuilder::infer_binary_intersection_type_comparison` where we currently short-circuit on every error.


We even have a test for this here, which asserts that there should be an error, but I think that is wrong: https://github.com/astral-sh/ruff/blob/6df10c638e3afed4a3fd9145d0353861e29d6acc/crates/ty_python_semantic/resources/mdtest/comparison/intersections.md?plain=1#L127

Ideally, we would want to distinguish between "operator not available" and "call to operator failed". The former should not be an error (if other intersection elements lead to a successful call), but the latter should probably always be an error?

---

_Label `help wanted` added by @sharkdp on 2025-05-22 13:19_

---

_Closed by @sharkdp on 2025-05-23 10:55_

---
