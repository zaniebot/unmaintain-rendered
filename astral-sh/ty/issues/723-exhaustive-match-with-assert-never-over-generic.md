```yaml
number: 723
title: "Exhaustive match with `assert_never()` over generic types does not work"
type: issue
state: closed
author: cam-matsui
labels: []
assignees: []
created_at: 2025-06-28T21:13:41Z
updated_at: 2025-06-30T07:37:44Z
url: https://github.com/astral-sh/ty/issues/723
synced_at: 2026-01-12T15:54:23Z
```

# Exhaustive match with `assert_never()` over generic types does not work

---

_@cam-matsui_

### Summary

example:

```py
from typing import assert_never, Literal
from dataclasses import dataclass

@dataclass
class V1[T1]:
    t: T1

@dataclass
class V2[T2]:
    t: T2

type V = V1 | V2
    

def my_func(v: V) -> bool:  # E: Function can implicitly return `None`, which is not assignable to return type `bool`
    match v:
        case V1():
            return True
        case V2():
            return False
        case _:
            assert_never(v)  # E: Argument does not have asserted type `Never`
```

mypy passes here; I would expect ty to pass as well given `v` is either a `V1` or `V2`

Playground link: https://play.ty.dev/a1c99b5b-d5e6-474d-9f00-08c6a613fcc0

### Version

_No response_

---

_Comment by @sharkdp on 2025-06-30 07:37_

Thank you for reporting this.

This is a duplicate of #99 

---

_Closed by @sharkdp on 2025-06-30 07:37_

---
