---
number: 8771
title: False negatives on PYI rules
type: issue
state: closed
author: AlexWaygood
labels:
  - rule
  - help wanted
assignees: []
created_at: 2023-11-19T21:54:55Z
updated_at: 2024-01-18T10:06:37Z
url: https://github.com/astral-sh/ruff/issues/8771
synced_at: 2026-01-07T13:12:15-06:00
---

# False negatives on PYI rules

---

_Issue opened by @AlexWaygood on 2023-11-19 21:54_

Ruff reports no errors on the following code snippet (saved as a `.pyi` stub file) with the command `ruff --select=PYI`, whereas flake8-pyi reports quite a few ;)

```py
import mypy_extensions
import typing
import typing_extensions

from typing import TypedDict, TypeVarTuple, Union
from typing_extensions import TypeAlias

DoublyNestedAlias: TypeAlias = Union[type[str], type[float] | type[bytes]]

class _UnusedTypedDict3(mypy_extensions.TypedDict):
    baz: dict[str, str]

_UnusedTypedDict4 = TypedDict("_UnusedTypedDict4", {"-": int, "def": str})
_UnusedTypedDict5 = typing_extensions.TypedDict("_UnusedTypedDict5", {"foo": bytes, "bar": str})
_P = typing_extensions.ParamSpec("_P")
_Ts = TypeVarTuple("_Ts")

type lowercase_alias = str | int
type _LooksLikeATypeVarT = str | int
type _Unused = str | int

def badfunc1(*args: typing.NoReturn) -> None: ...
def badfunc2(**kwargs: typing_extensions.NoReturn) -> None: ...
```

If I run `flake8 --extend-ignore=Y023,Y031,Y037` on the same file (with the latest version of the flake8-pyi installed), it reports the following violations:

```
sample.pyi:8:49: Y055 Multiple "type[Foo]" members in a union. Combine them into one, e.g. "type[float | bytes]".
sample.pyi:10:1: Y049 TypedDict "_UnusedTypedDict3" is not used
sample.pyi:13:1: Y049 TypedDict "_UnusedTypedDict4" is not used
sample.pyi:14:1: Y049 TypedDict "_UnusedTypedDict5" is not used
sample.pyi:15:1: Y018 ParamSpec "_P" is not used
sample.pyi:16:1: Y018 TypeVarTuple "_Ts" is not used
sample.pyi:18:1: Y042 Type aliases should use the CamelCase naming convention
sample.pyi:19:1: Y043 Bad name for a type alias (the "T" suffix implies a TypeVar)
sample.pyi:19:1: Y047 Type alias "_LooksLikeATypeVarT" is not used
sample.pyi:20:1: Y047 Type alias "_Unused" is not used
sample.pyi:22:15: Y050 Use "typing_extensions.Never" instead of "NoReturn" for argument annotations
sample.pyi:23:16: Y050 Use "typing_extensions.Never" instead of "NoReturn" for argument annotations
```

TODO:

- [x] Y018 false negatives on unused TypeVarTuples and ParamSpecs
- [x] Y042 false negatives on lowercase PEP-695 type aliases
- [x] Y043 false negatives on PEP-695 type aliases
- [x] Y047 false negatives on PEP-695 type aliases (https://github.com/astral-sh/ruff/pull/9566)
- [ ] Y049 false negatives on TypedDicts constructed using `mypy_extensions.TypedDict` rather than `typing.TypedDict` or `typing_extensions.TypedDict`
- [x] Y049 false negatives on TypedDicts constructed using call-based syntax (https://github.com/astral-sh/ruff/pull/9567)
- [x] Y050 false negatives on `*args` and `**kwargs` annotated using `typing.NoReturn`

---

_Comment by @AlexWaygood on 2023-11-19 21:56_

(Some of these are due to recent changes we've made in flake8-pyi -- e.g. the violations on lines 18-20 are because we recently extended our type alias rules to cover type aliases created using PEP-695 type statements.)

---

_Label `rule` added by @charliermarsh on 2023-11-19 22:58_

---

_Comment by @charliermarsh on 2023-11-19 22:58_

Thank you!

---

_Label `help wanted` added by @zanieb on 2023-11-20 16:16_

---

_Referenced in [astral-sh/ruff#8948](../../astral-sh/ruff/pulls/8948.md) on 2023-12-01 16:30_

---

_Referenced in [astral-sh/ruff#8966](../../astral-sh/ruff/pulls/8966.md) on 2023-12-02 14:58_

---

_Referenced in [astral-sh/ruff#9198](../../astral-sh/ruff/pulls/9198.md) on 2023-12-19 12:04_

---

_Referenced in [astral-sh/ruff#9566](../../astral-sh/ruff/pulls/9566.md) on 2024-01-17 16:41_

---

_Referenced in [astral-sh/ruff#9567](../../astral-sh/ruff/pulls/9567.md) on 2024-01-17 17:14_

---

_Comment by @AlexWaygood on 2024-01-18 10:06_

All but one of these are now ticked off 🥳

I'm closing this now, since the last one is sort-of in a category by itself, and I'm not sure we'd want to fix it. The obvious way to teach ruff to recognize `mypy_extensions.TypedDict` would be to have ruff recognize `mypy_extensions` as a "typing module" in the same way as `typing` or `typing_extensions`. But unlike `typing` and `typing_extensions`, `mypy_extensions` is, well, mypy-specific -- other type checkers accord it no special treatment. `mypy_extensions.TypedDict` specifically [is also deprecated](https://github.com/python/mypy_extensions/commit/e0c6670e05a87507d59b7d3a0aa2eec88e9813b0) in favour of `typing(_extensions).TypedDict` (though there's yet to be a release that includes the `DeprecationWarning`)

---

_Closed by @AlexWaygood on 2024-01-18 10:06_

---
