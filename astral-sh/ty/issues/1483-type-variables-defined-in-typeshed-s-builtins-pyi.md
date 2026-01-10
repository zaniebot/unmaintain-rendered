---
number: 1483
title: "Type variables defined in typeshed's `builtins.pyi` are usable in all files"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2025-11-05T15:06:54Z
updated_at: 2026-01-09T03:25:28Z
url: https://github.com/astral-sh/ty/issues/1483
synced_at: 2026-01-10T01:48:23Z
---

# Type variables defined in typeshed's `builtins.pyi` are usable in all files

---

_Issue opened by @AlexWaygood on 2025-11-05 15:06_

### Summary

We don't emit a diagnostic on this snippet... but we should, because `_T_co` is not defined 😆

```py
class SupportsNext:
    def __next__(self) -> _T_co:
        raise NotImplementedError
```

We infer `_T_co` as being available here because of its definition in `builtins.pyi` [here](https://github.com/python/typeshed/blob/07b69ab9e92dbe4ca55c48b1c23ebf34b41538f1/stdlib/builtins.pyi#L83). But that definition should be treated as private-to-the-stub, I think.

Possibly we should just filter out typevar definitions when falling back to the builtin scope in name lookup? The same could also be said for type-alias definitions and protocol definitions in `builtins.pyi`, but they're arguably both more useful and less confusing to have available (you might want to use them quoted in type annotations, e.g. `x: "_PositiveInteger"` wouldn't be an unreasonable type annotation.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-11-05 15:06_

---

_Comment by @AlexWaygood on 2025-11-06 15:21_

Pyrefly, mypy and pyright all filter out all "type-check-only" symbols (private type aliases, private typevars/paramspecs/typevartuples, private protocols) from the builtins fallback:

```py
reveal_type(_PositiveInteger)  # mypy, pyright, pyrefly: "`_PositiveInteger` is undefined"
reveal_type(_T_co)  # mypy, pyright, pyrefly: "`_T_co` is undefined"
reveal_type(_SupportsSynchronousAnext)  # mypy, pyright, pyrefly: "`_T_co` is undefined"
```

Mypy and pyrefly allow you to use these types if you explicitly import them, however, whereas pyright just views them as entirely private to the stub:

```py
# pyright: ❌, mypy/pyrefly: ✅
from builtins import _PositiveInteger, _T_co, _SupportsSynchronousAnext
```

I'd prefer to go with mypy/pyrefly's behaviour here:
- I'd rather not treat the `builtins` module differently to any other `.pyi` module when it comes to explicitly imported symbols
- I think it's often useful to import typeshed's protocols/aliases in `if TYPE_CHECKING` blocks, so I wouldn't want to forbid users from ever importing these type-check-only symbols in `.py` files

---

_Comment by @dhruvmanila on 2025-11-06 15:36_

This seems reasonable. I'm assuming that this would still take `__all__` into account like if the `_T_co` variable isn't included in the `__all__` of `module.pyi` then it would still raise a diagnostic.

---

_Comment by @AlexWaygood on 2025-11-06 15:59_

> I'm assuming that this would still take `__all__` into account like if the `_T_co` variable isn't included in the `__all__` of `module.pyi` then it would still raise a diagnostic.

We don't take `__all__` into account currently -- we only take `__all__` into account for symbols that are defined via imports in stub files. (These are always assumed to be private-to-the-stub unless they are included in `__all__` or they use a redundant alias.)

We could add a rule that says that these type-check-only symbols are treated as completely undefined if `__all__` exists and they're not included in `__all__`. But again, I think that would end up being annoying if you have an explicit import of one of these symbols in an `if TYPE_CHECKING` block. And I'd prefer to keep our rules for `.py` files and `.pyi` files as consistent as possible -- we generally allow you to import something from a `.py` file, even if that symbol is not included in the `__all__` of that `.py` file.

---

_Added to milestone `Stable` by @carljm on 2026-01-09 03:25_

---
