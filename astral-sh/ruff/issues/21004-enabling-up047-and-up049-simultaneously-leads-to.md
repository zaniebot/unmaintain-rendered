---
number: 21004
title: "Enabling UP047 and UP049 simultaneously leads to fixes introducing breaking changes with `--target-version=py314`, `from __future__ import annotations`, or on a `.pyi` file "
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-20T18:53:31Z
updated_at: 2026-01-06T15:28:17Z
url: https://github.com/astral-sh/ruff/issues/21004
synced_at: 2026-01-10T01:23:02Z
---

# Enabling UP047 and UP049 simultaneously leads to fixes introducing breaking changes with `--target-version=py314`, `from __future__ import annotations`, or on a `.pyi` file 

---

_Issue opened by @AlexWaygood on 2025-10-20 18:53_

### Summary

Consider the following snippet:

```py
from typing import TypeVar

_T = TypeVar("_T")

def f(self, x: _T) -> _T:
    return x
```

If you save it to a `foo.py` file, running `uvx ruff check foo.py --select="UP047,UP049,PYI018,F401" --fix --unsafe-fixes --target-version=py313 --diff` produces this output, which is great:

```diff
--- foo.py
+++ foo.py
@@ -1,6 +1,4 @@
-from typing import TypeVar
 
-_T = TypeVar("_T")
 
-def f(self, x: _T) -> _T:
+def f[T](self, x: T) -> T:
     return x
```

And if you run a type checker on the autofixed file, the type checker is happy. If you save it to a `foo.pyi` file, however (notice the change in file extension), the same command produces this output:

```diff
--- foo.pyi
+++ foo.pyi
@@ -2,5 +2,5 @@
 
 _T = TypeVar("_T")
 
-def f(self, x: _T) -> _T:
+def f[T](self, x: _T) -> _T:
     return x
```

And if you accept that autofix, mypy (for example) will correctly start complaining, because the function's annotations reference a type variable (`_T`) that is not declared in the function's type parameters:

```
foo.pyi:5: error: All type parameters should be declared ("_T" not declared)  [valid-type]
```

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-20 18:53_

---

_Label `fixes` added by @AlexWaygood on 2025-10-20 18:53_

---

_Comment by @ntBre on 2025-10-20 20:03_

Interesting. This looks like a bug in UP049. UP047 applies first resulting in:

```py
from typing import TypeVar

_T = TypeVar("_T")

def f[_T](self, x: _T) -> _T:
    return x
```

but then the rename goes wrong in UP049:

```
UP049 [*] Generic function uses private type parameters
 --> try.pyi:5:7
  |
3 | _T = TypeVar("_T")
4 |
5 | def f[_T](self, x: _T) -> _T:
  |       ^^
6 |     return x
  |
help: Rename type parameter to remove leading underscores
2 |
3 | _T = TypeVar("_T")
4 |
  - def f[_T](self, x: _T) -> _T:
5 + def f[T](self, x: _T) -> _T:
6 |     return x
```

---

_Comment by @dylwil3 on 2025-10-21 04:36_

The bad fix also reproduces on `.py` files with `from __future__ import annotations` and on Python 3.14+. The semantic model will not see that the annotation `_T` is a reference to the binding created by the type variable in this situation.

But I'm a little confused why that is happening, because I thought we would have visited the forward references by the time we did this renaming... Maybe we don't add them as references properly? Feels somewhat related to #20747 except that the rule is already a "deferred" rule.

---

_Comment by @AlexWaygood on 2025-10-23 13:26_

If I remember correctly, Ruff in some places emulates the (cursed! bad!) behaviour of `typing.get_type_hints` at runtime[^1], where global-scope names are preferred over local names when resolving stringified annotations:

```pycon
>>> from typing import get_type_hints
>>> X = int
>>> class Foo:
...     X = str
...     y: X
...     
>>> get_type_hints(Foo)
{'y': <class 'str'>}
>>> from __future__ import annotations
>>> class Foo:
...     X = str
...     y: X
...     
>>> get_type_hints(Foo)
{'y': <class 'int'>}
```

But even at runtime, `get_type_hints` has been taught[^2] to prefer local type-parameter scopes over global scopes when resolving stringified annotations:

```pycon
>>> from __future__ import annotations
>>> from typing import get_type_hints
>>> X = int
>>> class Foo[X]:
...     y: X
...     
>>> get_type_hints(Foo)
{'y': X}
```

So, even if we accept the (questionable) premise that it's correct for Ruff to emulate `typing.get_type_hints` here, it doesn't even seem to be doing that _entirely_ right currently.

[^1]: Everybody agrees that this is terrible but unfortunately it's very hard to change without breaking backwards compatibility
[^2]: Please don't look at the terrible hacks I had to add to typing.py in the Python stdlib to get this to work!!!

---

_Comment by @danparizher on 2025-10-26 19:09_

References to type parameters in string annotations are tracked, but their ranges are wrong.

In `.pyi` files (and files with `from __future__ import annotations`), annotations like `x: "_T"` are visited via `visit_deferred_string_type_definitions()` in `mod.rs`. The parser updates the semantic model, but the reference ranges point into the parsed string content, not the source file.

UP049’s `Renamer` then makes edits using those ranges and targets the wrong locations.

The fix requires mapping ranges from parsed annotations back to the source. Simple pattern-based rewrites are brittle in these contexts.

---

_Comment by @dylwil3 on 2025-10-27 15:05_

@danparizher Can you walk me through how you arrived at that conclusion? I can't seem to justify what you're saying here. There is a comment in the code for `visit_deferred_string_type_definitions()` that is vaguely related to what you're saying, here:

https://github.com/astral-sh/ruff/blob/db0e921db1466f8d23e28c27eaba19983a4367f0/crates/ruff_linter/src/checkers/ast/mod.rs#L2957-L2964

but it is only relevant if there are _nested_ string annotations, like `"list['str']"`. (And in any case I think the point of clearing the cache is to address that concern anyway?)

I think what's happening is that we are not properly handling references to the PEP695-style generic parameter when it is shadowing a type-variable assignment.

It's a little tricky to inspect the semantic model in action, but if you add some print-debugging then you can verify the following:

- For the snippet

```python
from __future__ import annotations
from typing import TypeVar

_T = TypeVar("_T")

def f(self, x: _T) -> _T:
    return x
```

we correctly see that each `_T` in the function statement is a reference to the binding `_T = TypeVar("_T")`.

- For the snippet

```python
from __future__ import annotations

def f[_T](self, x: _T) -> _T:
    return x
```

we correctly see that each `_T` in the function statement is a reference to the binding in `f[_T]`.

- For the snippet

```python
from __future__ import annotations
from typing import TypeVar

_T = TypeVar("_T")

def f[_T](self, x: _T) -> _T:
    return x
```

we think that each `_T` in the function statement is a reference to the assignment `_T = TypeVar("_T")`, rather than to the type parameter in `f[_T]`.

- To drive the point home, if we change `_T = TypeVar("_T")` to `_S = TypeVar("_S")` then we again get the correct references.

As far as I can tell, the ranges are irrelevant.




---

_Assigned to @dylwil3 by @dylwil3 on 2025-10-27 15:05_

---

_Comment by @danparizher on 2025-10-27 22:00_

Ah I didn't catch that detail, thank you for the pushback!

---

_Renamed from "Enabling UP047 and UP049 simultaneously on a `.pyi` file leads to fixes introducing breaking changes" to "Enabling UP047 and UP049 simultaneously with `--target-version=py314`, `from __future__ import annotations`, or in a `.pyi` file " by @AlexWaygood on 2026-01-06 15:28_

---

_Renamed from "Enabling UP047 and UP049 simultaneously with `--target-version=py314`, `from __future__ import annotations`, or in a `.pyi` file " to "Enabling UP047 and UP049 simultaneously leads to fixes introducing breaking changes with `--target-version=py314`, `from __future__ import annotations`, or on a `.pyi` file " by @AlexWaygood on 2026-01-06 15:28_

---
