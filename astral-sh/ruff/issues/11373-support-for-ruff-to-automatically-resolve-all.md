---
number: 11373
title: "Support for ruff to automatically resolve `__all__` statements."
type: issue
state: closed
author: leaver2000
labels: []
assignees: []
created_at: 2024-05-11T22:55:11Z
updated_at: 2024-05-12T08:28:21Z
url: https://github.com/astral-sh/ruff/issues/11373
synced_at: 2026-01-10T01:22:51Z
---

# Support for ruff to automatically resolve `__all__` statements.

---

_Issue opened by @leaver2000 on 2024-05-11 22:55_

It would be very useful and appreciated if there was an option to have `ruff` automatically resolve the `__all__` statement. A configuration might look something like...

```toml
[tool.ruff]
auto-all = ["**/__init__.py"]
```

From the [pep](https://peps.python.org/pep-0008/#public-and-internal-interfaces) regarding `__all__`.

> To better support introspection, modules should explicitly declare the names in their public API using the `__all__` attribute. Setting __all__ to an empty list indicates that the module has no public API.

That fix might look something like.

> Module level “dunders” should be placed after the module docstring but before any import statements except from __future__ imports [module-level-dunder-names](https://peps.python.org/pep-0008/#module-level-dunder-names)
```python
# app/__init__.py
"""docstring"""

from .core import my_function
```
running something along the lines of `ruff check auto-all="**/__init__.py" --fix app` would result in...

```python
# app/__init__.py
"""docstring"""
__all__ = ["my_function"]

from .core import my_function
```

Take this example from the [`dask/array/__init__.py`](https://github.com/dask/dask/blob/main/dask/array/__init__.py) where the `__all__`  is not set and the resulting complaint from `pylance`.

![image](https://github.com/astral-sh/ruff/assets/76945789/931a9c6e-8464-4042-b2cb-6c2bfb8efecb)




---

_Comment by @zanieb on 2024-05-12 05:03_

Hi! I think we're adding what you're looking for in https://github.com/astral-sh/ruff/pull/11314

---

_Closed by @leaver2000 on 2024-05-12 08:28_

---
