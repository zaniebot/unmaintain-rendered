---
number: 15532
title: "[PYI] Suggest using `typing.IO[bytes]` instead of `io.BytesIO`."
type: issue
state: open
author: randolf-scholz
labels:
  - rule
  - great writeup
assignees: []
created_at: 2025-01-16T14:59:01Z
updated_at: 2025-01-17T04:00:09Z
url: https://github.com/astral-sh/ruff/issues/15532
synced_at: 2026-01-07T13:12:16-06:00
---

# [PYI] Suggest using `typing.IO[bytes]` instead of `io.BytesIO`.

---

_Issue opened by @randolf-scholz on 2025-01-16 14:59_

When type hinting, particularly in function arguments, using the more flexible abstract class `typing.IO[bytes]` is often more appropriate than the concrete type `io.Bytes`. 

### Example

```python
from tempfile import Tempfile
import torch  # v2.5.1

with TemporaryFile() as file:
    model = torch.nn.Linear(3, 3)
    exported_model = torch.export.export(model, args=(torch.randn(3),))
    # "BufferedRandom" cannot be assigned to type "str | PathLike[Unknown] | BytesIO"
    torch.export.save(exported_model, file)  # ❌ typing error!
```

This works at runtime just fine, but type checkers will complain because `BufferedRandom`, the type of `file`, is not a subtype of `io.BytesIO`. The library should have annotated the method with `typing.IO[bytes]` instead. I propose the following rule:

### Rule

1. Suggest replacing `io.BytesIO` with `typing.IO[bytes]`/`typing.BinaryIO` and `io.TextIO` with `typing.IO[str]`/`typing.TextIO` inside **function argument annotations.**
2. Suggest replacing inside **function return annotations** only if they are methods of an abstract base class or Protocol.
3. Prefer `typing.IO[str]` and `typing.IO[bytes]` instead of `typing.TextIO` and `typing.BinaryIO` except when one of the members`buffer`, `encoding`, `errors`, `line_buffering`, `newlines` or `__enter__` is needed.

### References

- https://github.com/python/typing/discussions/829

---

_Comment by @AlexWaygood on 2025-01-16 15:11_

We've had a request for this before at flake8-pyi: see https://github.com/PyCQA/flake8-pyi/issues/463. I think it's a reasonable request; I agree that annotating a parameter with `io.BytesIO` is almost always a mistake from the user.

I'd be hesitant to recommend `typing.IO[bytes]` instead, though. It's a very large ABC, and lots of concrete IO classes are not recognised by type checkers as subtypes of the ABC. You're often better off going with a handrolled protocol that narrowly specifies exactly which IO methods you need.

---

_Label `rule` added by @AlexWaygood on 2025-01-16 15:11_

---

_Referenced in [pytorch/pytorch#144976](../../pytorch/pytorch/issues/144976.md) on 2025-01-16 17:03_

---

_Comment by @randolf-scholz on 2025-01-16 18:24_

It's unfortunate that the standard library doesn't provide a sensible `Protocol`, but since type checkers consider `io.BytesIO <: IO[bytes]` this would allow strictly more arguments.

So if the library author does not want to introduce an extra Protocol, then `typing.IO[bytes]` seems like the best bet, and is already widely used: https://github.com/search?q=%22IO%5Bbytes%5D%22+language%3APython&type=code&l=Python

---

_Comment by @AlexWaygood on 2025-01-16 20:05_

I'd be okay with an error message that said something like "Use `IO[bytes]` or a custom protocol", I just don't want to make it seem like `IO[bytes]` is the _only_ option -- because it's often not the best option, even if it's a convenient one :-)

---

_Comment by @randolf-scholz on 2025-01-16 20:21_

Another aspect of this is what to recommend for branching against `IO`-like interface. For example, given

```python
def save(name_or_buffer: str | os.PathLike[str] | io.BytesIO, data) -> None:
    if isinstance(name_or_buffer, io.BytesIO):
        ...  # use the `.write()` interface
    else:
        ....  # initialize some writer for the filename/path
```

If we recommend the user to use `IO[bytes]` in the annotation, they will be tempted to replace the `io.BytesIO` in the `isinstance` as well, but this is incorrect. At the very least, it should be `isinstance(x, IO | io.IOBase)`, as many of the buffer classes in the stdlib do not subclass `typing.IO`. On the other hand, `io.BytesIO` can be too strict. (like for `tempfile.TemporaryFile`).

Of course, using a runtime-checkable protocol is less error-prone in this regard.



---

_Label `great writeup` added by @dhruvmanila on 2025-01-17 04:00_

---

_Referenced in [pytorch/pytorch#144994](../../pytorch/pytorch/pulls/144994.md) on 2025-01-17 10:32_

---

_Referenced in [microsoft/yardl#241](../../microsoft/yardl/issues/241.md) on 2025-09-24 14:53_

---
