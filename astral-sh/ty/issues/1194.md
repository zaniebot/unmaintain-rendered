```yaml
number: 1194
title: "report error when ambiguous `FunctionType` as `Enum` value"
type: issue
state: open
author: KotlinIsland
labels:
  - wish
  - enums
assignees: []
created_at: 2025-09-17T05:57:21Z
updated_at: 2025-11-18T16:10:37Z
url: https://github.com/astral-sh/ty/issues/1194
synced_at: 2026-01-10T01:58:59Z
```

# report error when ambiguous `FunctionType` as `Enum` value

---

_Issue opened by @KotlinIsland on 2025-09-17 05:57_

### Summary

```py
from enum import Enum

def f() -> object:
    return lambda: 1

class E(Enum):
    a = f()  # expect error: `object` can potentially be `FunctionType`, wrap this value with `enum.member` to enforce expected runtime behaviour

E.a.name  # fails at runtime 
```
[playground](https://play.ty.dev/fdd093de-9c24-4e1c-989d-a3cb5379deaa)

`FunctionType` becomes a non-member, but currently ty assumes a super type of `FunctionType` is a member, use `enum.member` to override this and make it a member


### Version

_No response_

---

_Renamed from "report error when ambigious `FunctionType` as `Enum` value" to "report error when ambiguous `FunctionType` as `Enum` value" by @AlexWaygood on 2025-09-17 06:23_

---

_Label `wish` added by @AlexWaygood on 2025-09-17 06:23_

---

_Comment by @sharkdp on 2025-09-17 07:08_

Thank you for reporting this.

Our current distinction between members and non-members follows [this section of the typing spec](https://typing.python.org/en/latest/spec/enums.html#defining-members) closely. For example, we do treat "Methods, callables, descriptors (including properties), and nested classes" as non-members. I don't see anything in the spec that would suggest that (proper) supertypes of `FunctionType` should also be treated as non-members. And if we change the body of `f` in your example to return `1` instead of `lambda: 1`, everything would work fine at runtime. So I'm not sure if `object` should really be excluded as a potential member type?

---

_Comment by @KotlinIsland on 2025-09-18 00:39_

the issue is that it's ambigious if the value will be a member or not, we can't say with certainty what the behaviour will be at runtime, so it is a union of `member | non-member`, to be sound we should either:
1. show a warning when the membership is ambigious
2. derive a union of `member | T` (where `T` is the type)

doing anything otherwise would be equivalent to saying:

```py
def f() -> int | str
    return "f"

a: int = f()  # show no error?
a + 1  # runtime error
```
- If we change the body of `f` in this example to return `1` instead of `"f"`, everything would work fine at runtime

---

_Comment by @sharkdp on 2025-09-18 07:33_

That makes sense, but I guess my stronger argument was that members-vs-non-members is described in great detail in the typing spec, but there is nothing that says that items of type `object` should not be considered members. Now that could be an oversight in the spec, in which case we should attempt to improve it. Or there might be a good reason why it's not included.

I think I agree with you that an item of type `object` is more likely to be a mistake, so I guess we could try to see what happens (across the ecosystem) if we simply make `object` non-members?

---

_Comment by @KotlinIsland on 2025-09-18 09:49_

i think it is most certainly an oversight in the spec

i think any super type of `FunctionType` should become either `Unknown` or `member | non-member`

---

_Label `enums` added by @AlexWaygood on 2025-09-23 19:58_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
