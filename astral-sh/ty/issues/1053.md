```yaml
number: 1053
title: "Give submodules precedence over module-level `__getattr__`"
type: issue
state: closed
author: Avasam
labels:
  - needs-decision
  - runtime semantics
assignees: []
created_at: 2025-08-19T16:31:03Z
updated_at: 2025-11-03T20:24:03Z
url: https://github.com/astral-sh/ty/issues/1053
synced_at: 2026-01-10T02:06:24Z
```

# Give submodules precedence over module-level `__getattr__`

---

_Issue opened by @Avasam on 2025-08-19 16:31_

### Summary

Sorry if duplicate of a more generalized issue, the error is kinda precise and I couldn't find an equivalent issue already open.

Take the following MRO:
```py
from typing import reveal_type
from PySide6 import QtCore, QtWidgets

reveal_type(QtCore)
reveal_type(QtWidgets)
reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
reveal_type(QtWidgets.QFileDialog)
```

`uvx --with PySide6 ty check` (PySide6@6.9.1 as of writing this)

Results:

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files
info[revealed-type]: Revealed type
 --> example.py:4:13
  |
2 | from PySide6 import QtCore, QtWidgets
3 |
4 | reveal_type(QtCore)
  |             ^^^^^^ `list[str]`
5 | reveal_type(QtWidgets)
6 | reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
  |

info[revealed-type]: Revealed type
 --> example.py:5:13
  |
4 | reveal_type(QtCore)
5 | reveal_type(QtWidgets)
  |             ^^^^^^^^^ `list[str]`
6 | reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
7 | reveal_type(QtWidgets.QFileDialog)
  |

error[unresolved-attribute]: Type `list[str]` has no attribute `Qt`
 --> example.py:6:13
  |
4 | reveal_type(QtCore)
5 | reveal_type(QtWidgets)
6 | reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
  |             ^^^^^^^^^
7 | reveal_type(QtWidgets.QFileDialog)
  |
info: rule `unresolved-attribute` is enabled by default

info[revealed-type]: Revealed type
 --> example.py:6:13
  |
4 | reveal_type(QtCore)
5 | reveal_type(QtWidgets)
6 | reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
  |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `Unknown`
7 | reveal_type(QtWidgets.QFileDialog)
  |

error[unresolved-attribute]: Type `list[str]` has no attribute `QFileDialog`
 --> example.py:7:13
  |
5 | reveal_type(QtWidgets)
6 | reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
7 | reveal_type(QtWidgets.QFileDialog)
  |             ^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

info[revealed-type]: Revealed type
 --> example.py:7:13
  |
5 | reveal_type(QtWidgets)
6 | reveal_type(QtCore.Qt.WindowType.FramelessWindowHint)
7 | reveal_type(QtWidgets.QFileDialog)
  |             ^^^^^^^^^^^^^^^^^^^^^ `Unknown`
  |

Found 6 diagnostics
```



### Version

ty 0.0.1-alpha.18 (d697cc092 2025-08-14) AND ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Renamed from "PySide6 modules are seen as `list[str]`" to "PySide6 modules are seen as `list[str]` since Alpha 18" by @Avasam on 2025-08-19 16:31_

---

_Renamed from "PySide6 modules are seen as `list[str]` since Alpha 18" to "PySide6 modules are seen as `list[str]` since alpha.18" by @Avasam on 2025-08-19 16:31_

---

_Label `runtime semantics` added by @carljm on 2025-08-19 17:21_

---

_Label `needs-decision` added by @carljm on 2025-08-19 17:22_

---

_Renamed from "PySide6 modules are seen as `list[str]` since alpha.18" to "Consider giving submodules precedence over module-level `__getattr__`?" by @carljm on 2025-08-19 17:22_

---

_Comment by @carljm on 2025-08-19 17:22_

Thanks for the report!

The problem here is that PySide6 has an `__init__.py` that [has a module-level `__getattr__` function which is annotated to return `list[str]`](https://github.com/pyside/pyside-setup/blob/dev/sources/pyside6/PySide6/__init__.py.in#L122), and it doesn't have any explicit import of its submodules. In that situation the `__getattr__` takes precedence over looking for submodules, and we model that runtime precedence accurately. (We also model accurately that if the `__init__.py` itself explicitly imports the submodule, that does take precedence over the module `__getattr__`.)

(We added support for module-level `__getattr__` in ty alpha.18.)

Pyright and mypy both appear to model the precedence here in a way that doesn't match runtime -- they always prefer a submodule over a module `__getattr__`. This will lead them to incorrect inference in some cases, but perhaps this is an intentional divergence from the runtime semantics? I'm not sure.

The reason this works at runtime for PySide6 is that their module `__getattr__` only returns for the name `__all__`, and otherwise raises `AttributeError`, causing the import system to fall back to looking for a submodule. But there's no (standardized, specified) way to annotate the `__getattr__` to tell us that it raises `AttributeError` for all but a certain set of names.

One possible solution here would be for PySide6 to provide a top level `__init__.pyi` that would omit the `__getattr__`, since it has no value for type checkers and is just a runtime optimization for lazy `__all__`.

Another possible solution would be to reach consensus (on discuss.python.org) for adding language to the typing spec establishing a semantics for `__getattr__` where the key/name argument can be annotated with a more limited union of literal string types to communicate "this `__getattr__` method will raise AttributeError on any argument other than these". Then PySide6 could annotate its `__getattr__` with an argument type of `Literal["__all__"]`, and we would ignore the `__getattr__` for any other name. (In fact we already implement these semantics, but neither mypy nor pyright do.) I think this is pretty reasonable: an argument annotation does communicate "I will raise if you call me with an argument type outside my annotated one."

I'll leave this issue open for now, to collect feedback on whether it will be important for us to follow mypy and pyright in their divergence from the runtime semantics of module-level `__getattr__`.

---

_Comment by @Avasam on 2025-08-19 19:31_

Thanks a lot for the full analysis! I'm not too surprised the PySide6 stubs themselves are cause for issue, they have their own custom type-stubs generator, and I often had to raise issues for incorrectness in their stubs, or ask for improvements.

In this case it might be a harder sell because of how both mypy an pyright, two well-established type checkers, handle their stubs. The new kid on the block (ty) is the one that doesn't work.

Hopefully a consensus can be reached !

---

_Comment by @AlexWaygood on 2025-08-19 19:43_

> One possible solution here would be for PySide6 to provide a top level `__init__.pyi` that would omit the `__getattr__`, since it has no value for type checkers and is just a runtime optimization for lazy `__all__`.

I think it probably doesn't need quite such an invasive solution? This would probably fix the issue, I think:

```diff
- def __getattr__(name: str) -> list[str]:
-     if name == "__all__":
-         global __all__
-         __all__ = _find_all_qt_modules()
-         return __all__
-     raise AttributeError(f"module '{__name__}' has no attribute '{name}' :)")
+ TYPE_CHECKING = False
+
+ if not TYPE_CHECKING:
+     def __getattr__(name: str) -> list[str]:
+         if name == "__all__":
+             global __all__
+             __all__ = _find_all_qt_modules()
+             return __all__
+         raise AttributeError(f"module '{__name__}' has no attribute '{name}' :)")

---

_Comment by @carljm on 2025-09-10 17:17_

I think we should just follow mypy and pyright here -- this issue isn't worth the incompatibility. The only case in which the mypy/pyright approach gives incorrect results is if someone is intentionally shadowing a submodule with a package `__getattr__`, and if there's a good use case for doing this, it doesn't seem to have come up yet.

I think it makes sense to assume that an argument incompatible with the annotated parameter type of `__getattr__` will result in `AttributeError`, so I'm glad we support that -- and in an ideal world, I believe that's how this scenario should be handled. But without a clearer use case and real-world demand, I think it will be hard to convince mypy and pyright that they should also support that, and then convince real-world users of package `__getattr__` to annotate their `__getattr__` functions that way. And we have more important things to focus on.

---

_Renamed from "Consider giving submodules precedence over module-level `__getattr__`?" to "Give submodules precedence over module-level `__getattr__`" by @carljm on 2025-09-10 17:17_

---

_Added to milestone `GA` by @carljm on 2025-09-10 17:17_

---

_Comment by @saucoide on 2025-10-28 12:35_

another realworld example, in `anyio` this was recently added to handle warnings for typo

https://github.com/agronholm/anyio/pull/938

```python
# __init__.py
# ...
def __getattr__(attr: str) -> type[BrokenWorkerInterpreter]:
    """Support deprecated aliases."""
    if attr == "BrokenWorkerIntepreter":
        import warnings

        warnings.warn(
            "The 'BrokenWorkerIntepreter' alias is deprecated, use 'BrokenWorkerInterpreter' instead.",
            DeprecationWarning,
            stacklevel=2,
        )
        return BrokenWorkerInterpreter

    raise AttributeError(f"module {__name__!r} has no attribute {attr!r}")
```

making submodules type like a `BrokenWorkerInterpreter`

```python
from anyio import to_thread
to_thread.current_default_thread_limiter()  # Type `type[BrokenWorkerInterpreter]` has no attribute `current_default_thread_limiter`
```


---

_Comment by @Avasam on 2025-11-03 01:05_

> But there's no (standardized, specified) way to annotate the `__getattr__` to tell us that it raises `AttributeError` for all but a certain set of names.

I would imagine combining `overload` and `Literal` like the following to make sense, but yeah, not supported by any type-checker that I know of:
```py
@overload
def __getattr__(name: Literal["foo"]) -> str: ...
@overload
def __getattr__(name: Literal["bar"]) -> int: ...
# No matching overloads means you fall back as if `__getattr__` raises `AttributeError`
```

if the above was accepted/standardized, you could do the following:

The PySide example could be annotated using:
```py
def __getattr__(name: Literal["__all__"]) -> list[str]:
```

@saucoide 's anyio as:
```py
def __getattr__(name: Literal["BrokenWorkerIntepreter"]) -> type[BrokenWorkerInterpreter]:
```

---

This also wouldn't work because it should just get simplified back to the original signature (and I don't think special-casing `__getattr__` for this is a good idea):

```py
def __getattr__(name: str) -> NoReturn | list[str]:
```


---

_Closed by @carljm on 2025-11-03 20:24_

---
