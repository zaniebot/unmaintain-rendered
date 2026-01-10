```yaml
number: 368
title: file.write for files opened in binary mode
type: issue
state: closed
author: heiner
labels:
  - bug
assignees: []
created_at: 2025-05-13T19:25:52Z
updated_at: 2025-11-12T01:52:11Z
url: https://github.com/astral-sh/ty/issues/368
synced_at: 2026-01-10T02:06:24Z
```

# file.write for files opened in binary mode

---

_Issue opened by @heiner on 2025-05-13 19:25_

### Summary

Given this input `test.py`:

```
f = open("myfile", "ab+")
f.write(b"mybytes")
```

Running `ty` results in

```
$ uvx ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to bound method `write` is incorrect
 --> /Users/heiner/src/test.py:2:9
  |
1 | f = open("myfile", "ab+")
2 | f.write(b"mybytes")
  |         ^^^^^^^^^^ Expected `str`, found `Literal[b"mybytes"]`
  |
info: Function defined here
   --> stdlib/_io.pyi:125:9
    |
123 |     def __next__(self) -> str: ...  # type: ignore[override]
124 |     def detach(self) -> BinaryIO: ...
125 |     def write(self, s: str, /) -> int: ...
    |         ^^^^^       ------ Parameter declared here
126 |     def writelines(self, lines: Iterable[str], /) -> None: ...  # type: ignore[override]
127 |     def readline(self, size: int = -1, /) -> str: ...  # type: ignore[override]
    |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Label `bug` added by @carljm on 2025-05-13 19:29_

---

_Comment by @carljm on 2025-05-13 19:31_

Thanks for the report!

Root cause here is lack of support for `typing.TypeAlias`, because `_typeshed.OpenTextMode` is defined as a `TypeAlias`, which causes us to pick the wrong overload for `open`.

---

_Comment by @AlexWaygood on 2025-10-06 15:52_

Thanks for the report! I'm closing this in favour of https://github.com/astral-sh/ty/issues/544, since the fix for this issue is #544

---

_Closed by @AlexWaygood on 2025-10-06 15:52_

---

_Comment by @carljm on 2025-11-12 01:52_

Confirmed that this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
