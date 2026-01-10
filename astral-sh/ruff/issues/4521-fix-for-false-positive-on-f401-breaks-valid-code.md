```yaml
number: 4521
title: Fix for false positive on F401 breaks valid code
type: issue
state: closed
author: RUrlus
labels:
  - bug
assignees: []
created_at: 2023-05-19T08:19:38Z
updated_at: 2023-06-06T00:48:55Z
url: https://github.com/astral-sh/ruff/issues/4521
synced_at: 2026-01-10T11:09:47Z
```

# Fix for false positive on F401 breaks valid code

---

_Issue opened by @RUrlus on 2023-05-19 08:19_

* ruff v0.0.269
* CPython 3.10.9

**Issue**

ruff flags a false positive F401 for imports that are used in `__all__` that is guarded by a conditional statement.
Running ruff with `--fix` on will result in broken imports.

**MRE**

```python
import random


def some_dependency_check():
    return random.uniform(0.0, 1.0) > 0.49999

if some_dependency_check():
    import math

    __all__ = ["math"]
else:
    __all__ = []

```

```shell
-> ruff mre.py
mre.py:9:12: F401 [*] `math` imported but unused
Found 1 error.
[*] 1 potentially fixable with the --fix option.
``` 


---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-19 12:34_

---

_Comment by @charliermarsh on 2023-05-19 12:50_

So, can and should avoid flagging this as unused...

I do feel obligated to mention that you should try to avoid using conditionally-defined `__all__` assignments :) They will cause issues with all sorts of tooling, since many conditions are undecidable at "compile" time, e.g., Pyright treats `x` as undefined if you use a wildcard import on this file:

```py
x = 1

if x > 0:
    __all__ = ["x"]
else:
    __all__ = []
```


---

_Label `bug` added by @charliermarsh on 2023-05-19 12:50_

---

_Comment by @JonathanPlasse on 2023-05-19 12:53_

We should maybe merge all `__all__`s.

---

_Comment by @RUrlus on 2023-05-19 13:45_

@charliermarsh Thanks for the quick response!

In the example you gave, `x` is an ast.Constant and thus pyright could/ should conclude that the `else` branch is unreachable *at import time* .

> I do feel obligated to mention that you should try to avoid using conditionally-defined __all__ assignments

I agree, but I don't know how to avoid it in the pattern where I encountered this.  I want to support different backends/types for dispatching without making all of them dependencies:

```python
from foo.backends import pandas, spark

@singledispatch
def strip_punctuation(data):
    raise NotImplementedError(f"no backend with support for type {type(data)} was found")

if pandas._supported:
    strip_punctuation.register(pandas.strip_punctuation)

if spark._supported:
    strip_punctuation.register(spark.strip_punctuation)
```

Where the imports are guarded, `foo.backends.pandas.__init__.py`:
```python
_supported = can_import("pandas")
if _supported:
    from foo.backends.pandas.string import strip_punctuation
    
    __all__ = ["strip_punctuation"]
else:
    __all__ = []
```

---

_Comment by @charliermarsh on 2023-05-19 14:03_

@JonathanPlasse - Yeah I think that's probably what we need to do.

---

_Comment by @JonathanPlasse on 2023-05-19 14:05_

~I can work on it if you want.~

---

_Comment by @MichaReiser on 2023-05-19 14:26_

> @JonathanPlasse - Yeah I think that's probably what we need to do.

I'm not sure if merging gives us the desired result in all situations:

* When testing for public docstrings -> Merging them is what we want
* When testing for invalid imports -> Merging is unsound because it then wont flag imports that may not exist at runtime. A better approach for this use case is to explicitly determine the "conditional" exports and potentially warn about them (but not flag any that are in both sets)
* others?

---

_Comment by @charliermarsh on 2023-05-19 14:38_

Yeah that's true. Another option could be to iterate over all `Export` bindings (even those that are overridden) when we do these checks.

---

_Comment by @charliermarsh on 2023-05-24 03:43_

I'm looking into this.

---

_Closed by @charliermarsh on 2023-06-06 00:48_

---
