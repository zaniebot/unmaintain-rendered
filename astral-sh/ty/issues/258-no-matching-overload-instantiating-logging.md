```yaml
number: 258
title: “no-matching-overload” instantiating logging.StreamHandler
type: issue
state: closed
author: turian
labels:
  - bug
  - generics
assignees: []
created_at: 2025-05-08T00:56:25Z
updated_at: 2025-05-19T15:45:42Z
url: https://github.com/astral-sh/ty/issues/258
synced_at: 2026-01-12T15:54:22Z
```

# “no-matching-overload” instantiating logging.StreamHandler

---

_@turian_

### Summary


```
error: lint\:no-matching-overload: No overload of bound method `__init__` matches arguments
```

for this minimal example:

```
import logging
h = logging.StreamHandler()          # ← error
```

## Why this is unexpected

* The CPython docs and runtime accept a zero-argument constructor that defaults to `sys.stderr`.

* typeshed’s `logging/__init__.pyi` reflects that signature:

  ```
    class StreamHandler(Handler):
        def __init__(self, stream: IO[str] | None = ...) -> None: ...
  ```

* Ty’s own stub—`red_knot/builtins/logging.pyi`—still exposes the old overloads that take `filename` | `PathLike` and therefore rejects the common no-argument form.

Because of that divergence, projects that pass mypy/ruff/mkdocs etc. must add:

```
handler = logging.StreamHandler()          # ty: ignore
```

and then hit issue #130 when the code is split across lines.

## Repro

1. `echo 'import logging; logging.StreamHandler()' > t.py`
2. `ty check t.py`

## Expected

No diagnostics.

## Actual

`lint:no-matching-overload`

## Proposed fix

Import logging and update the `StreamHandler` overloads in Ty’s stdlib stubs to match typeshed (i.e., make the first positional argument an optional `IO[str]`, default `None`).

---

This bug is separate from #130 (“unused suppression for type: ignore”) – that issue only appears because we’re forced to ignore this stub mismatch.


### Version

0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Closed by @turian on 2025-05-08 00:57_

---

_Comment by @my1e5 on 2025-05-08 14:10_

Is this issue solved?

https://play.ty.dev/e7169c70-7716-4ed4-aa4e-cb2e74510acf

```
import logging
import sys

logging.StreamHandler(stream=sys.stdout)
```
```
$ uvx ty check foo.py
error: lint:no-matching-overload: No overload of bound method `__init__` matches arguments
 --> foo.py:4:1
  |
2 | import sys
3 |
4 | logging.StreamHandler(stream=sys.stdout)
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: `lint:no-matching-overload` is enabled by default

Found 1 diagnostic
```


---

_Reopened by @carljm on 2025-05-08 14:12_

---

_Comment by @carljm on 2025-05-08 14:18_

Ty doesn't have its own stubs, it uses typeshed.

I suspect the issue here is that we are actually failing to match on the `self` annotations of those overloads, for some generics-related reason.

(This issue also highlights that our diagnostics on a failed call to an overloaded function need some attention: https://github.com/astral-sh/ty/issues/274)

---

_Label `bug` added by @carljm on 2025-05-08 14:18_

---

_Label `generics` added by @carljm on 2025-05-08 14:18_

---

_Renamed from "logging.StreamHandler() flagged as “no-matching-overload” – ty stub requires  filename while typeshed & stdlib allow zero-arg constructor" to "“no-matching-overload” instantiating logging.StreamHandler" by @carljm on 2025-05-08 14:20_

---

_Comment by @BlinkyStitt on 2025-05-14 23:06_

i also see this error with this simple code:

```
with tempfile.TemporaryDirectory() as temp_dir:
    pass
```

---

_Closed by @dcreager on 2025-05-19 15:45_

---

_Closed by @dcreager on 2025-05-19 15:45_

---
