---
number: 10358
title: "Don't strip an ellipsis (`...`) from a function's body - this has special meaning to type-checkers"
type: issue
state: closed
author: rsokl
labels:
  - rule
assignees: []
created_at: 2024-03-12T14:13:07Z
updated_at: 2024-03-15T03:55:58Z
url: https://github.com/astral-sh/ruff/issues/10358
synced_at: 2026-01-07T13:12:15-06:00
---

# Don't strip an ellipsis (`...`) from a function's body - this has special meaning to type-checkers

---

_Issue opened by @rsokl on 2024-03-12 14:13_

Before `ruff`:

```python
if TYPE_CHECKING:
    def compress(bytes: Sequence[int], target: int) -> list[int]:
        """Docs"
        ...   # <- this tells type-checkers to treat this function like a stub and ignore its body
```

After `ruff`:

```python
if TYPE_CHECKING:
    def compress(bytes: Sequence[int], target: int) -> list[int]:  # type: ignore
        """Docs"""
        # Without ... the type-checker assumes the return type is None
        # thus the type-checker marks the return-type annotation as an error.
```

The ellipses signals to the type-checker (e.g. pyright) that function is acting as a stub and the body should be ignored. In the latter case, the type-checker thinks that the function returns `None` and thus marks the return-type as an error. 

I.e. without the ellipses, you have to either: provide a fake function body or `# type: ignore` the function's signature, which would mask actual type-errors in the signature.


---

_Label `rule` added by @charliermarsh on 2024-03-12 14:15_

---

_Comment by @zanieb on 2024-03-12 14:23_

Hm we talked about this previously re protocols

- https://github.com/astral-sh/ruff/issues/8756

Should we make another exception for `if TYPE_CHECKING` blocks?

---

_Referenced in [astral-sh/ruff#8756](../../astral-sh/ruff/issues/8756.md) on 2024-03-12 14:41_

---

_Comment by @rsokl on 2024-03-12 14:43_

> Should we make another exception for if TYPE_CHECKING blocks?

That would be good.

Treating `pass` and `...` as being the same seems weird to me. They are definitely not the same thing.

---

_Comment by @zanieb on 2024-03-13 14:30_

This should be relatively easy if someone is interested in it.

---

_Referenced in [astral-sh/ruff#10413](../../astral-sh/ruff/pulls/10413.md) on 2024-03-14 21:55_

---

_Closed by @charliermarsh on 2024-03-15 03:55_

---
