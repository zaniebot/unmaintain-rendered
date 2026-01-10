```yaml
number: 519
title: Assertions on object attributes do not refine later checks
type: issue
state: closed
author: adamh-oai
labels: []
assignees: []
created_at: 2025-05-26T23:55:52Z
updated_at: 2025-05-27T00:07:16Z
url: https://github.com/astral-sh/ty/issues/519
synced_at: 2026-01-10T02:34:10Z
```

# Assertions on object attributes do not refine later checks

---

_Issue opened by @adamh-oai on 2025-05-26 23:55_

### Summary

I'm sure there's real type system terms to describe this, but should be clear enough from an example:

```
class A:
    value: str | None

def foo(zzz: A):
    if zzz.value and len(zzz.value):
        print(zzz.value)

# no error
def bar(zzz: A):
    value = zzz.value
    if value and len(value):
        print(zzz.value)
```

This produces:

```
$ uvx ty@latest check .
error[invalid-argument-type]: Argument to function `len` is incorrect
 --> nullable.py:6:26
  |
5 | def foo(zzz: A):
6 |     if zzz.value and len(zzz.value):
  |                          ^^^^^^^^^ Expected `Sized`, found `str | None`
7 |         print(zzz.value)
  |
info: Function defined here
    --> stdlib/builtins.pyi:1500:5
     |
1498 | def isinstance(obj: object, class_or_tuple: _ClassInfo, /) -> bool: ...
1499 | def issubclass(cls: type, class_or_tuple: _ClassInfo, /) -> bool: ...
1500 | def len(obj: Sized, /) -> int: ...
     |     ^^^ ---------- Parameter declared here
1501 |
1502 | license: _sitebuiltins._Printer
     |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic

```

This is *technically* correct in that the value could have changed behind your back between the test and use, but fairly common so wanted to check if it's intended.  In my unscientific sampling of the errors ty reports in our codebase, this seems to be the most common cause.

### Version

ty 0.0.1-alpha.7

---

_Closed by @AlexWaygood on 2025-05-27 00:06_

---

_Comment by @AlexWaygood on 2025-05-27 00:07_

Thanks for the report! The good news is that this is a feature we're very actively working on. It should hopefully be ready soon 🚀

---
