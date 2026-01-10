```yaml
number: 314
title: "List concatenation not resolving correctly to `list[str]` but to `list[_T]`"
type: issue
state: closed
author: ion-elgreco
labels:
  - bug
  - generics
assignees: []
created_at: 2025-05-10T17:23:35Z
updated_at: 2025-05-12T17:48:55Z
url: https://github.com/astral-sh/ty/issues/314
synced_at: 2026-01-10T02:34:09Z
```

# List concatenation not resolving correctly to `list[str]` but to `list[_T]`

---

_Issue opened by @ion-elgreco on 2025-05-10 17:23_

### Summary

```python
def print_list(value: list[str]) -> None:
    print(value)


existing_list = ["foo"]
print_list(
    existing_list
    + [
        "bar",
    ],
)
```

```python
  --> test.py:7:5
   |
 5 |   existing_list = ["foo"]
 6 |   print_list(
 7 | /     existing_list
 8 | |     + [
 9 | |         "bar",
10 | |     ],
   | |_____^ Expected `list[str]`, found `list[_T]`
11 |   )
   |
info: Function defined here
 --> test.py:1:5
  |
1 | def print_list(value: list[str]) -> None:
  |     ^^^^^^^^^^ ---------------- Parameter declared here
2 |     print(value)
  |
info: `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

_No response_

---

_Renamed from "List concatenation not resolving correctly to `list[str]` but to `list[T_]`" to "List concatenation not resolving correctly to `list[str]` but to `list[_T]`" by @ion-elgreco on 2025-05-10 17:23_

---

_Label `generics` added by @AlexWaygood on 2025-05-10 17:26_

---

_Label `bug` added by @AlexWaygood on 2025-05-10 17:26_

---

_Comment by @hauntsaninja on 2025-05-10 23:48_

I think ty just doesn't support any list concatenation right now:
```
error[unsupported-operator]: Operator `+` is unsupported between objects of type `list[int]` and `list[int]`
  --> test1.py:14:12
   |
13 | def f4(x: list[int], y: list[int]):
14 |     return x + y
   |            ^^^^^
   |
info: `unsupported-operator` is enabled by default
```

---

_Closed by @dcreager on 2025-05-12 17:48_

---
