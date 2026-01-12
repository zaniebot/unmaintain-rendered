```yaml
number: 12945
title: PyUpgrade rule UP007 does not work on forward reference string types
type: issue
state: open
author: goodspark
labels:
  - question
assignees: []
created_at: 2024-08-17T00:28:36Z
updated_at: 2024-08-19T17:21:02Z
url: https://github.com/astral-sh/ruff/issues/12945
synced_at: 2026-01-12T15:54:52Z
```

# PyUpgrade rule UP007 does not work on forward reference string types

---

_@goodspark_

Keywords: forward reference, pyupgrade, UP007

Python 3.12, also 3.10
Ruff 0.6.1, also 0.5.7
Command: `ruff check .`

Code:

Python file
```py
x: Optional['Something']  # expect UP007 to trigger here -> x: 'Something | None'
y: Union[int, 'Something']  # expect UP007 to trigger here -> y: 'Something | int'
x2: Optional[Something]  # correctly caught by UP007
y2: Union[int, Something]  # correctly caught by UP007
class Something:
  pass
```

pyproject.toml
```toml
[project]
# As long as this is >=3.10 or any subset, like ==3.10, this bug will occur.
requires-python = ">=3.12"

[tool.ruff.lint]
extend-select = ["UP"]
extend-safe-fixes = ["UP007"]
```


---

_Renamed from "PyUpgrade rule UP007 do not work on forward reference string types" to "PyUpgrade rule UP007 does not work on forward reference string types" by @goodspark on 2024-08-17 00:28_

---

_Comment by @charliermarsh on 2024-08-18 22:25_

I believe that's because `'Something' | None` is not a valid type annotation -- it will error at runtime. 

---

_Comment by @charliermarsh on 2024-08-18 22:26_

If you add `from __future__ import annotations`, we do correctly flag all four, since the type annotations are then deferred.

---

_Label `question` added by @charliermarsh on 2024-08-18 22:26_

---

_Comment by @charliermarsh on 2024-08-18 22:26_

So I think this is working as intended, AFAICT.

---

_Comment by @goodspark on 2024-08-19 17:21_

I guess another fix I was envisioning was `"Something | None"` (note both Something and None are in strings).

---
