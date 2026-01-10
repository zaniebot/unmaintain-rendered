```yaml
number: 307
title: "can't infer `pydantic.dataclasses.dataclass`"
type: issue
state: closed
author: trim21
labels:
  - imports
  - dataclasses
assignees: []
created_at: 2025-05-10T12:52:46Z
updated_at: 2025-05-13T14:59:13Z
url: https://github.com/astral-sh/ty/issues/307
synced_at: 2026-01-10T02:34:09Z
```

# can't infer `pydantic.dataclasses.dataclass`

---

_Issue opened by @trim21 on 2025-05-10 12:52_

### Summary

There are third party data classes use `@dataclass_transform` decoder on their `dataclass` decoder, for example, `pydantic.dataclasses.dataclass`, which is not support by ty.

```python
    @dataclass_transform(field_specifiers=(dataclasses.field, Field, PrivateAttr))
    @overload
    def dataclass(
...
```

ty won't add add a `__init__` for this:


```python
from pydantic import dataclasses


@dataclasses.dataclass()
class Config:
    name: str
    age: int


Config(name="hello", age=18)

```

```text
error[unknown-argument]: Argument `name` does not match any known parameter of bound method `__init__`
  --> e.py:10:8
   |
10 | Config(name="hello", age=18)
   |        ^^^^^^^^^^^^
   |
info: `unknown-argument` is enabled by default

error[unknown-argument]: Argument `age` does not match any known parameter of bound method `__init__`
  --> e.py:10:22
   |
10 | Config(name="hello", age=18)
   |                      ^^^^^^
   |
info: `unknown-argument` is enabled by default
```

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Renamed from "missing support for `typing_extensions.dataclass_transform`" to "missing support for `typing.dataclass_transform`" by @trim21 on 2025-05-10 12:54_

---

_Label `dataclasses` added by @AlexWaygood on 2025-05-10 17:58_

---

_Comment by @abhijeetbodas2001 on 2025-05-11 07:11_

I beleive this is something that should've been handled as part of https://github.com/astral-sh/ruff/pull/17835. Let me try to investigate.

---

_Comment by @trim21 on 2025-05-11 08:12_

emmm, sorry . I find that pydantic is actually using `typing_extensions.dataclass_transform`.

If `typing.dataclass_transform` works then I file this issue with wrong topic.

---

_Renamed from "missing support for `typing.dataclass_transform`" to "missing support for `typing_extensions.dataclass_transform.dataclass_transform`" by @trim21 on 2025-05-11 08:33_

---

_Renamed from "missing support for `typing_extensions.dataclass_transform.dataclass_transform`" to "missing support for `typing_extensions.dataclass_transform`" by @trim21 on 2025-05-11 08:33_

---

_Comment by @sharkdp on 2025-05-11 08:50_

Thank you for reporting this. This is not related to missing `dataclass_transform` support, it's actually related to a bug (#261), which will be fixed by https://github.com/astral-sh/ruff/pull/18005. We currently infer `Never` for `pydantic.dataclasses.dataclass`:

```py
from pydantic import dataclasses

reveal_type(dataclasses.dataclass)  # Never
```

When checking the example from the original post above using the version from https://github.com/astral-sh/ruff/pull/18005, everything works as expected.

---

_Comment by @trim21 on 2025-05-11 08:53_

thanks!

---

_Label `imports` added by @AlexWaygood on 2025-05-11 09:01_

---

_Renamed from "missing support for `typing_extensions.dataclass_transform`" to "can't infer `pydantic.dataclasses.dataclass`" by @trim21 on 2025-05-11 09:05_

---

_Closed by @sharkdp on 2025-05-13 14:59_

---

_Closed by @sharkdp on 2025-05-13 14:59_

---
