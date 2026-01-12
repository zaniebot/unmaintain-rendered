```yaml
number: 405
title: os.path functions get wrong type assignments.
type: issue
state: closed
author: Michallote
labels:
  - bug
assignees: []
created_at: 2025-05-15T09:40:57Z
updated_at: 2025-11-12T01:38:31Z
url: https://github.com/astral-sh/ty/issues/405
synced_at: 2026-01-12T15:54:23Z
```

# os.path functions get wrong type assignments.

---

_@Michallote_

### Summary

Minimum code fail:

```python
#fail.py
import os

def get_rel(output_directory: str) -> str:
    assert os.path.exists(
        output_directory
    ), f"Output directory '{output_directory}' does not exist. Create the folder and try again."
    output_directory = os.path.relpath(output_directory)
    return output_directory
```

And running in the terminal:

```bash
$ uvx ty check fail.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-assignment]: Object of type `bytes` is not assignable to `str`
  --> fail.py:9:5
   |
 7 |         output_directory
 8 |     ), f"Output directory '{output_directory}' does not exist. Create the folder and try again."
 9 |     output_directory = os.path.relpath(output_directory)
   |     ^^^^^^^^^^^^^^^^
10 |     return output_directory
   |
info: rule `invalid-assignment` is enabled by default

Found 1 diagnostic
```

commenting os.path.exists does not change the outcome.

### Version

ty 0.0.1-alpha.2

---

_Comment by @MichaReiser on 2025-05-15 09:44_

Playground: https://play.ty.dev/79aa2c89-ec65-438f-91e6-81d9a6341275

---

_Label `bug` added by @sharkdp on 2025-05-15 11:28_

---

_Label `Protocols` added by @sharkdp on 2025-05-15 11:28_

---

_Label `Protocols` removed by @sharkdp on 2025-05-15 11:28_

---

_Comment by @sharkdp on 2025-05-15 11:32_

Thank you for reporting this. `os.path.relpath` is overloaded in typeshed:
```pyi
@overload
def relpath(path: LiteralString, start: LiteralString | None = None) -> LiteralString: ...
@overload
def relpath(path: BytesPath, start: BytesPath | None = None) -> bytes: ...
@overload
def relpath(path: StrPath, start: StrPath | None = None) -> str: ...
```

We mistakenly use the second overload here which returns `bytes`. The reason we do this is that we don't understand the `BytesPath` type alias yet (since it's a `typing.TypeAlias` alias, not a PEP 695 `type` alias):

```pyi
BytesPath: TypeAlias = bytes | PathLike[bytes]
```

see https://github.com/astral-sh/ty/issues/221 which keeps track of our type alias support.

---

_Comment by @Michallote on 2025-05-15 15:58_

But the operation overloading should return StrPath if my input is typed as a string no? Or the BytesPath makes an union with str too therefore defaulting to it first as it doesn't check the last one?

---

_Comment by @jelle-openai on 2025-05-15 16:31_

Because ty doesn't understand `TypeAlias`, it treats both `BytesPath` and `StrPath` as essentially Any and picks the first one that matches.

---

_Comment by @AlexWaygood on 2025-10-06 15:55_

Thanks again for the report! I'm closing this in favour of https://github.com/astral-sh/ty/issues/544, since the fix for this will just be to implement #544

---

_Closed by @AlexWaygood on 2025-10-06 15:55_

---

_Comment by @carljm on 2025-11-12 01:38_

Confirmed that this is fixed by https://github.com/astral-sh/ruff/pull/21394

---
