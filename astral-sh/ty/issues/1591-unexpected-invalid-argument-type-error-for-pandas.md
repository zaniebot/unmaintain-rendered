```yaml
number: 1591
title: Unexpected invalid-argument-type error for pandas DataFrame with columns
type: issue
state: closed
author: knutae
labels: []
assignees: []
created_at: 2025-11-19T11:16:02Z
updated_at: 2025-11-19T20:53:02Z
url: https://github.com/astral-sh/ty/issues/1591
synced_at: 2026-01-10T01:58:59Z
```

# Unexpected invalid-argument-type error for pandas DataFrame with columns

---

_Issue opened by @knutae on 2025-11-19 11:16_

### Summary

The following code produces an invalid-argument-type error

```python
import pandas

def frametest():
    return pandas.DataFrame([[1], [2]], columns=["a", "b"])
```

Output:

```
$ uvx ty check pandas_columns.py
error[invalid-argument-type]: Argument to bound method `__init__` is incorrect
 --> pandas_columns.py:4:41
  |
3 | def frametest():
4 |     return pandas.DataFrame([[1], [2]], columns=["a", "b"])
  |                                         ^^^^^^^^^^^^^^^^^^ Expected `ExtensionArray | ndarray[@Todo, dtype[Any]] | Index | ... omitted 4 union elements`, found `list[Unknown | str]`
  |
info: Method defined here
   --> .venv/lib/python3.11/site-packages/pandas/core/frame.py:698:9
    |
696 |     # Constructors
697 |
698 |     def __init__(
    |         ^^^^^^^^
699 |         self,
700 |         data=None,
701 |         index: Axes | None = None,
702 |         columns: Axes | None = None,
    |         --------------------------- Parameter declared here
703 |         dtype: Dtype | None = None,
704 |         copy: bool | None = None,
    |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

This was tested with the latest pandas version, 2.3.3.

Note that it works with ty version 0.0.1-alpha.26, but fails with 0.0.1-alpha.27.

### Version

0.0.1-alpha.27

---

_Comment by @carljm on 2025-11-19 20:15_

Thanks for the report! It looks like this error is in fact correct, though we definitely need to provide better diagnostics here (tracked in https://github.com/astral-sh/ty/issues/163). The best fix on your end is to simply install the `pandas-stubs` stubs package, which provides better type annotations for pandas users.

Pyright gives the same error here, and provides a great diagnostic to explain the root cause:

```
  /Users/carlmeyer/projects/ruff-examples/pandastest/main.py:3:13 - information: Type of "pandas.DataFrame.__init__" is "(self: DataFrame, data: Unknown | None = None, index: ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | Series | SequenceNotStr[Unknown] | range | None = None, columns: ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | Series | SequenceNotStr[Unknown] | range | None = None, dtype: ExtensionDtype | str | dtype[Any] | type[str] | type[complex] | type[bool] | type[object] | None = None, copy: bool | None = None) -> None"
  /Users/carlmeyer/projects/ruff-examples/pandastest/main.py:6:49 - error: Argument of type "list[str]" cannot be assigned to parameter "columns" of type "Axes | None" in function "__init__"
    Type "list[str]" is not assignable to type "Axes | None"
      "list[str]" is not assignable to "ExtensionArray"
      "list[str]" is not assignable to "ndarray[_AnyShape, dtype[Any]]"
      "list[str]" is not assignable to "Index"
      "list[str]" is not assignable to "Series"
      "list[str]" is incompatible with protocol "SequenceNotStr[Unknown]"
        "index" is an incompatible type
          Type "(value: str, start: SupportsIndex = 0, stop: SupportsIndex = sys.maxsize, /) -> int" is not assignable to type "(value: Any, /, start: int = 0, stop: int = ...) -> int"
    ... (reportArgumentType)
```

Ultimately the bug is a single mis-placed character in the signature of the `index` method of pandas' `SequenceNotStr` type. In [the 2.3.x branch of pandas, that method has this signature](https://github.com/pandas-dev/pandas/blob/2.3.x/pandas/_typing.py#L140):

```py
def index(self, value: Any, /, start: int = 0, stop: int = ...) -> int: ...
```

Note that the `/` comes after `value` parameter, but before `start` and `stop`, meaning that `start` and `stop` may be passed as keywords.

But [in typeshed, the builtin `list` type has an `index` method with this signature](https://github.com/python/typeshed/blob/main/stdlib/builtins.pyi#L1117):

```py
def index(self, value: _T, start: SupportsIndex = 0, stop: SupportsIndex = sys.maxsize, /) -> int: ...
```

Here the `/` comes after all the parameters, meaning they are all positional-only; none may be passed as a keyword argument. That discrepancy makes `list` not assignable to the `SequenceNotStr` protocol.

This pandas bug has been fixed [in the main branch of pandas](https://github.com/pandas-dev/pandas/blob/main/pandas/_typing.py#L124), and [does not occur in pandas-stubs](https://github.com/pandas-dev/pandas-stubs/blob/main/pandas-stubs/_typing.pyi#L106) (which is why installing `pandas-stubs` fixes the problem). 

So closing this as not a bug in ty, though it definitely provides a great example of why we need https://github.com/astral-sh/ty/issues/163, as well as https://github.com/astral-sh/ty/issues/1537.

---

_Closed by @carljm on 2025-11-19 20:15_

---

_Comment by @carljm on 2025-11-19 20:53_

(Oh, and the reason this "worked" on alpha 26 and failed on alpha 27 was simply that alpha 27 added proper validation of method assignability on protocols. So it was really an improvement in ty that caused us to catch this true positive error!)

---
