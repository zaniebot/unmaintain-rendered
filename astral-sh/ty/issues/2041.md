```yaml
number: 2041
title: Type Narrowing on match with multiple case
type: issue
state: open
author: Polandia94
labels:
  - narrowing
assignees: []
created_at: 2025-12-17T21:27:50Z
updated_at: 2026-01-08T19:06:47Z
url: https://github.com/astral-sh/ty/issues/2041
synced_at: 2026-01-10T01:56:41Z
```

# Type Narrowing on match with multiple case

---

_Issue opened by @Polandia94 on 2025-12-17 21:27_

### Summary

Type Narrowing on a match class with multiple cases is not working correctly. 
If there is only one case is working fine. On pylance, and mypy the same example works

playground link https://play.ty.dev/30d20c1a-1fd3-4fdc-a495-675c0eb2dee8
on mypy https://mypy-play.net/?mypy=latest&python=3.12&gist=c5e206614ccee154996e4363afe72549

```py
class BaseClass: ...
class Implementation(BaseClass): ...
class ImplementationWithValue(BaseClass):
   value: int
class OtherImplementationWithValue(BaseClass):
   value: int
def get_instance() -> BaseClass:
    return ImplementationWithValue()
instance = get_instance()
match instance:
    case ImplementationWithValue():
        pass
    case OtherImplementationWithValue():
        pass
    case _:
        raise Exception()
reveal_type(instance) # BaseClass on ty, but ImplementationWithValue | OtherImplementationWithValue on pylance and mypy
instance.value # Object of type BaseClass has no attribute value
match instance:
    case ImplementationWithValue():
        pass
    case _:
        raise Exception()
reveal_type(instance) # ImplementationWithValue
instance.value # works
```

### Version

_No response_

---

_Comment by @carljm on 2025-12-17 21:30_

Thanks for the report!

This may be related to #690, but I'm not entirely sure whether it's that, or a limitation in our match handling, so I'll leave it open as a separate issue.

---

_Added to milestone `Stable` by @carljm on 2025-12-17 21:30_

---

_Label `narrowing` added by @AlexWaygood on 2025-12-17 21:31_

---
