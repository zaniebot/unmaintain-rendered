```yaml
number: 460
title: Context manager contextlib.nullcontext causes diagnostic
type: issue
state: closed
author: behrenhoff
labels:
  - bug
assignees: []
created_at: 2025-05-20T12:36:37Z
updated_at: 2025-05-20T12:46:09Z
url: https://github.com/astral-sh/ty/issues/460
synced_at: 2026-01-12T15:54:23Z
```

# Context manager contextlib.nullcontext causes diagnostic

---

_@behrenhoff_

### Summary

Consider the following simple code:

```
from contextlib import nullcontext


def foo() -> None:
    with nullcontext():
        print("ok")
```

Then:
```
$ ty --version
ty 0.0.1-alpha.5

$ ty check ty-test.py                    
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[no-matching-overload]: No overload of bound method `__init__` matches arguments
 --> ty-test.py:5:10
  |
4 | def foo() -> None:
5 |     with nullcontext():
  |          ^^^^^^^^^^^^^
6 |         print("ok")
  |
info: First overload defined here
   --> stdlib/contextlib.pyi:188:13
    |
186 |         enter_result: _T
187 |         @overload
188 |         def __init__(self: nullcontext[None], enter_result: None = None) -> None: ...
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
189 |         @overload
190 |         def __init__(self: nullcontext[_T], enter_result: _T) -> None: ...  # pyright: ignore[reportInvalidTypeVarUse]  #11780
    |
info: Possible overloads for bound method `__init__`:
info:   (self: nullcontext[None], enter_result: None = None) -> None
info:   (self: nullcontext[_T], enter_result: _T) -> None
info: rule `no-matching-overload` is enabled by default

Found 1 diagnostic
```

I am not exactly sure what is wrong here but the code above should not produce a diagnostic. It seems it has to do with `contextlib.nullcontext` as the disagnostic does not appear when using `pytest.raises` instead.

### Version

0.0.1-alpha.5

---

_Comment by @sharkdp on 2025-05-20 12:44_

I think this has been fixed by https://github.com/astral-sh/ruff/pull/18155 (and https://github.com/astral-sh/ruff/pull/18204), we just haven't release a new version yet. I can not reproduce this on `main` (you can try it on [our playground](https://play.ty.dev/9a4f6dd4-af0d-4a60-9704-8939fb550b25), which runs the latest version on `main`)

---

_Label `bug` added by @sharkdp on 2025-05-20 12:45_

---

_Closed by @sharkdp on 2025-05-20 12:46_

---
