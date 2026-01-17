```yaml
number: 2540
title: Typed dictionary narrowing not working
type: issue
state: closed
author: gaborbernat
labels:
  - typeddict
assignees: []
created_at: 2026-01-17T01:08:04Z
updated_at: 2026-01-17T01:20:56Z
url: https://github.com/astral-sh/ty/issues/2540
synced_at: 2026-01-17T02:11:18Z
```

# Typed dictionary narrowing not working

---

_@gaborbernat_

### Summary

```python
from typing import TypedDict, final

class InnerA(TypedDict):
    foo: str

class InnerB(TypedDict):
    bar: str

@final
class OuterWithA(TypedDict):
    inner: InnerA

@final
class OuterWithB(TypedDict):
    inner: InnerB

InnerUnion = OuterWithA | OuterWithB

@final
class Response(TypedDict):
    data: InnerUnion

# This should work - the dict literal matches OuterWithA structure
response: Response = {
    "data": {
        "inner": {
            "foo": "hello"
        }
    }
}
```

The error shows ty can't narrow the union type based on the dict literal structure, even though {"inner": {"foo": "hello"}} clearly matches OuterWithA and not OuterWithB:

```
error[invalid-argument-type]: Invalid argument to key "data" with declared type `OuterWithA | OuterWithB` on TypedDict `Response`
  --> /tmp/ty_repro.py:24:22
   |
23 |    # This should work - the dict literal matches OuterWithA structure
24 |    response: Response = {
   |  _______________________-
25 | |      "data": {
   | | _____------__^
   | ||     |
   | ||     key has declared type `OuterWithA | OuterWithB`
26 | ||         "inner": {
27 | ||             "foo": "hello"
28 | ||         }
29 | ||     }
   | ||_____^ value of type `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
30 | |  }
   | |__- TypedDict `Response`
   |
info: Item declaration
  --> /tmp/ty_repro.py:21:5
   |
19 | @final
20 | class Response(TypedDict):
21 |     data: InnerUnion
   |     ---------------- Item declared here
22 |
23 | # This should work - the dict literal matches OuterWithA structure
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```


### Platform

macos

### Version

0.0.11

### Python version

3.14

---

_Label `bug` added by @gaborbernat on 2026-01-17 01:08_

---

_Comment by @AlexWaygood on 2026-01-17 01:16_

I cannot reproduce this in the [playground](https://play.ty.dev/f4136d80-7916-4b88-8fc3-04806580abf9). Are you using the latest version of ty?

---

_Label `bug` removed by @AlexWaygood on 2026-01-17 01:16_

---

_Label `typeddict` added by @AlexWaygood on 2026-01-17 01:16_

---

_Comment by @AlexWaygood on 2026-01-17 01:20_

Sorry, I see you said that you're using 0.0.11. I can reproduce it with that version, but not on the latest release (0.0.12). So the fix here is to upgrade to the latest version of ty :-)

---

_Closed by @AlexWaygood on 2026-01-17 01:20_

---
