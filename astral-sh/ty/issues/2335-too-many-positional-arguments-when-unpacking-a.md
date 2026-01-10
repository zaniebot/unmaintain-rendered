```yaml
number: 2335
title: "\"Too many positional arguments\" when unpacking a sequence for function parameters."
type: issue
state: closed
author: Boon-in-Oz
labels:
  - calls
assignees: []
created_at: 2026-01-05T04:12:16Z
updated_at: 2026-01-05T08:47:53Z
url: https://github.com/astral-sh/ty/issues/2335
synced_at: 2026-01-10T01:56:41Z
```

# "Too many positional arguments" when unpacking a sequence for function parameters.

---

_Issue opened by @Boon-in-Oz on 2026-01-05 04:12_

### Summary

I found a minor bug. 
With a function like this:
```py
def test(a: int, b: int, c: int) -> None:
    """A test function to demonstrate unpacking arguments."""
```
The following works fine, though ty's inline preview identifies the parameter assignment incorrectly:
```py
param = (1, 2, 3)
test(*param)
param2 = [4, 5, 6]
test(*param2)
test(param2[0], param2[1], 7)
```
but these examples both incorrectly trigger the ty error `Too many positional arguments to function `test`: expected 3, got 4ty[too-many-positional-arguments](https://ty.dev/rules#too-many-positional-arguments)`
```py
test(*param2[:2], 7)

param3 = [8, 9]
test(*param3, 10)
```
while this incorrectly does not trigger any errors:
```py
test(*param3)
```

Here's a screenshot of the VS Code window, showing the incorrect inline hints. Not a big deal, but I assume related; ty appears to assume that any unpacked sequence matches all parameters perfectly:

<img width="476" height="189" alt="Image" src="https://github.com/user-attachments/assets/94d39365-cee1-431f-a7a0-310188b790f6" />

### Version

2025.80.0

---

_Comment by @Boon-in-Oz on 2026-01-05 04:31_

Ah. This problem is limited to lists. Tuples work correctly. I expect that might be as good as it can get. It would be nice if it could see the length of a slice, or if it would make the same assumptions about intent when extra parameters are supplied after an unpacked list (ie that I supplied a list with the correct length).

---

_Label `calls` added by @AlexWaygood on 2026-01-05 07:51_

---

_Comment by @dhruvmanila on 2026-01-05 08:47_

Thanks for the report!

This is same as https://github.com/astral-sh/ty/issues/1584 (similar to https://github.com/astral-sh/ty/issues/2251) where unpacking a list eagerly assigns to all the remaining parameters because ty doesn't know how many elements will the argument unpack into.

I'll fold this into #1584.

---

_Closed by @dhruvmanila on 2026-01-05 08:47_

---
