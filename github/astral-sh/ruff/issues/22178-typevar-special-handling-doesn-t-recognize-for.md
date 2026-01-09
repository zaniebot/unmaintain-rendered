---
number: 22178
title: "TypeVar special handling doesn't recognize ** for ARG001"
type: issue
state: closed
author: henryiii
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-12-24T16:40:18Z
updated_at: 2025-12-31T16:07:57Z
url: https://github.com/astral-sh/ruff/issues/22178
synced_at: 2026-01-07T13:12:16-06:00
---

# TypeVar special handling doesn't recognize ** for ARG001

---

_Issue opened by @henryiii on 2025-12-24 16:40_

### Summary

This example produces a false positive:

```python
import typing

def f(
    *args: object,
    default: object = None,  # noqa: ARG001
    **kwargs: object,
) -> None:
    typing.TypeVar(*args, **kwargs)

__all__ = ["f"]
```

```console
$ ruff check --isolated --select=ARG tmp.py
ARG001 Unused function argument: `kwargs`
 --> tmp.py:6:7
  |
4 |     *args: object,
5 |     default: object = None,  # noqa: ARG001
6 |     **kwargs: object,
  |       ^^^^^^
7 | ) -> None:
8 |     typing.TypeVar(*args, **kwargs)
  |

Found 1 error.
```

To handle Python 3.13's new `default=` argument to `TypeVar`, a pattern including the above is helpful. See https://github.com/scikit-build/scikit-build-core/pull/1202.


### Version

_No response_

---

_Comment by @ntBre on 2025-12-24 17:31_

Thanks for the report! It looks like we indeed don't visit variadic keyword arguments at all in `TypeVar`s, which seems like a bug for sure.

https://github.com/astral-sh/ruff/blob/e3498121b4700f5a9dce9bf6d72af4bc411e262d/crates/ruff_linter/src/checkers/ast/mod.rs#L1863-L1877

I think we should visit this, and possibly some of  our other `typing::Callable` variants here, as `non_type_definition`s so that references to their bindings are actually recorded. This seems analogous to our change for `NamedTuple` in https://github.com/astral-sh/ruff/pull/8810.

---

_Label `bug` added by @ntBre on 2025-12-24 17:31_

---

_Label `help wanted` added by @ntBre on 2025-12-24 17:31_

---

_Comment by @ValdonVitija on 2025-12-26 14:02_

@ntBre Is anyone working on this, if not I would for sure like to give this one a try!

---

_Comment by @ntBre on 2025-12-26 14:56_

Not that I know of, go for it!

---

_Assigned to @ValdonVitija by @ntBre on 2025-12-26 14:56_

---

_Referenced in [astral-sh/ruff#22214](../../astral-sh/ruff/pulls/22214.md) on 2025-12-26 22:26_

---

_Closed by @ntBre on 2025-12-31 16:07_

---
