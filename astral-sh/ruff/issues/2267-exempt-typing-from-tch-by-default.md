```yaml
number: 2267
title: "Exempt `typing` from TCH by default?"
type: issue
state: closed
author: scop
labels: []
assignees: []
created_at: 2023-01-27T17:08:40Z
updated_at: 2023-01-27T23:19:46Z
url: https://github.com/astral-sh/ruff/issues/2267
synced_at: 2026-01-12T15:54:42Z
```

# Exempt `typing` from TCH by default?

---

_@scop_

ruff 0.0.236

It would seem to me that `typing` should be exempted from TCH by default.

For example:
```python
from typing import Any

def foo() -> Any:
    return 1
```

Unless I'm missing something, use of the type checking block that satisfies this check requires importing `TYPE_CHECKING` from `typing`, which loads `typing`, thus nullifying the usefulness for this for `typing` imports.

```
>>> import sys
>>> "typing" in sys.modules
False
>>> from typing import TYPE_CHECKING
>>> "typing" in sys.modules
True
```

flake8-type-checking's README points to two "examples in the wild", both of which contain `typing` imports, neither of which places them in the `TYPE_CHECKING` block:
- https://github.com/python-poetry/poetry/blob/714c09dd845c58079cff3f3cbedc114dff2194c9/src/poetry/factory.py#L1:L33
- https://github.com/snok/asgi-correlation-id/blob/main/asgi_correlation_id/middleware.py#L1:L12

flake8-type-checking also contains this note:
https://github.com/snok/flake8-type-checking/blob/main/flake8_type_checking/checker.py#L601-L603
...along with some other conditions regarding `typing` import aliasing and `typing.cast` imports above that commentary. I haven't dug into them nor if ruff currently has any corresponding functionality.

---

_Comment by @ngnpope on 2023-01-27 17:57_

There is a small advantage to moving, e.g. `from typing import Any, Generic, ParamSpec, TypeAlias, TypeVar, ...`, into a `TYPE_CHECKING` block: Reducing the amount of cruft littering the module globals at runtime.

---

_Comment by @charliermarsh on 2023-01-27 17:58_

We could make it part of `exempt-modules` by default, that way it's configurable.

---

_Comment by @ngnpope on 2023-01-27 18:02_

Also, is this happening because you have `strict = true`? You can also set `exempt-modules = ["typing"]` if you want to prevent this happening...

---

_Comment by @scop on 2023-01-27 18:41_

No `strict = true` here. I understand I could exempt it for myself, I just think it would be a better default for most cases, therefore suggesting it here.

---

_Comment by @charliermarsh on 2023-01-27 18:56_

Yeah I think the least surprising thing would be to exempt typing and typing extensions by default.

---

_Comment by @scop on 2023-01-27 19:28_

`typing_extensions` is a bit different, as I think it can be entirely put within `TYPE_CHECKING` and the import actually avoided at runtime. Therefore I'd personally not exempt it by default. But no strong opinions on that here.

---

_Comment by @charliermarsh on 2023-01-27 23:00_

Yeah makes sense. I'll add `typing`.

---

_Closed by @charliermarsh on 2023-01-27 23:19_

---
