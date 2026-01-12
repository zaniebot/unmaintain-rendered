```yaml
number: 928
title: "Capture with `as` in `match` statement breaks type narrowing"
type: issue
state: closed
author: mthuurne
labels:
  - bug
  - control flow
assignees: []
created_at: 2025-08-02T23:46:48Z
updated_at: 2025-08-04T18:13:51Z
url: https://github.com/astral-sh/ty/issues/928
synced_at: 2026-01-12T15:54:24Z
```

# Capture with `as` in `match` statement breaks type narrowing

---

_@mthuurne_

### Summary

[testcase in playground](https://play.ty.dev/42bb391c-e742-4b01-adf1-7a91cee18a26)

If I check the following code:
```py
from typing import Never, NoReturn


def bad_type(value: Never) -> NoReturn:
    raise TypeError(type(value))


def f(value: str | int) -> None:
    match value:
        case int() as x:
            print("integer:", x)
        case str():
            print("string")
        case _:
            bad_type(value)
```
ty reports:
```
error[invalid-argument-type]: Argument to function `bad_type` is incorrect
  --> narrowing_to_never.py:15:22
   |
13 |             print("string")
14 |         case _:
15 |             bad_type(value)
   |                      ^^^^^ Expected `Never`, found `int`
   |
info: Function defined here
 --> narrowing_to_never.py:4:5
  |
4 | def bad_type(value: Never) -> NoReturn:
  |     ^^^^^^^^ ------------ Parameter declared here
5 |     raise TypeError(type(value))
  |
info: rule `invalid-argument-type` is enabled by default
```
I would expect it to not report any issues, as the default case is unreachable if a properly typed value is passed.

When the capture (`as x`) is removed, no issues are reported. It seems that the capture disables the type narrowing.


### Version

ty 0.0.1-alpha.16

---

_Label `bug` added by @sharkdp on 2025-08-04 06:44_

---

_Label `control flow` added by @sharkdp on 2025-08-04 06:44_

---

_Closed by @sharkdp on 2025-08-04 18:13_

---
