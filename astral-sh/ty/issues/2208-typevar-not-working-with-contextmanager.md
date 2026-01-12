```yaml
number: 2208
title: TypeVar not working with contextmanager
type: issue
state: closed
author: igortg
labels:
  - generics
assignees: []
created_at: 2025-12-24T17:53:48Z
updated_at: 2025-12-24T18:58:32Z
url: https://github.com/astral-sh/ty/issues/2208
synced_at: 2026-01-12T15:54:26Z
```

# TypeVar not working with contextmanager

---

_@igortg_

### Summary

It seems TypeVar is not recognized when used with `contextmanager` decorator.

```python
R = TypeVar("R")


class A:
    def row(self):
        return 1

def factory(c: R) -> R:
    return c

@contextmanager
def factory_context(c: R) -> Generator[R, None]:
    yield c

a = factory(A())  # OK

with factory_context(A()) as inst:  # Expected `R@factory_context`, found `A`
    inst.row()                      # Object of type `R@factory_context` has no attribute `row` 
```

https://play.ty.dev/8ca471f8-40c6-4330-a31f-a27c724a4818

### Version

0.0.6

---

_Comment by @ntBre on 2025-12-24 18:08_

I think this may be a duplicate of https://github.com/astral-sh/ty/issues/2030, which looks similar to me but uses PEP 695 type parameters.

---

_Label `generics` added by @AlexWaygood on 2025-12-24 18:57_

---

_Comment by @AlexWaygood on 2025-12-24 18:58_

> I think this may be a duplicate of [#2030](https://github.com/astral-sh/ty/issues/2030), which looks similar to me but uses PEP 695 type parameters.

Yes, looks like it. Thanks @ntBre!

---

_Closed by @AlexWaygood on 2025-12-24 18:58_

---
