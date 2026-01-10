```yaml
number: 9324
title: Add option to move module level dunders to top of file
type: issue
state: open
author: nstarman
labels:
  - rule
assignees: []
created_at: 2023-12-30T21:47:23Z
updated_at: 2023-12-31T17:25:20Z
url: https://github.com/astral-sh/ruff/issues/9324
synced_at: 2026-01-10T11:09:51Z
```

# Add option to move module level dunders to top of file

---

_Issue opened by @nstarman on 2023-12-30 21:47_

In https://pep8.org/#module-level-dunder-names it says that `__all__` "should be placed after the module docstring but before any import statements except from `__future__` imports".
It would be great to have an isort-style rule that implements the PEP8 suggestion.
There are definitely cases when `__all__` shouldn't be moved to above the imports, e.g. when it is dynamically defined from the imports, but if `__all__` contains only strings then it should be safe to move.

```python
import x, y, z

__all__ = ["x", "y", "z"]  # safe to move
```

```python
from x import *
from . import x

__all__ = x.__all__  # not safe to move
```

---

_Label `rule` added by @charliermarsh on 2023-12-31 12:40_

---

_Comment by @Skylion007 on 2023-12-31 16:20_

For reference, isort already has this an option so we might just be able to add it to the isort rules: https://github.com/PyCQA/isort/commit/44e3b5d179da5033ce23f796b014e74f3a3259cd

---

_Comment by @charliermarsh on 2023-12-31 16:48_

Does that open sort the _members_ of `__all__`, or move the `__all__` itself? I couldn't tell from the PR.

---

_Comment by @alchzh on 2023-12-31 16:50_

> For reference, isort already has this an option so we might just be able to add it to the isort rules: [PyCQA/isort@44e3b5d](https://github.com/PyCQA/isort/commit/44e3b5d179da5033ce23f796b014e74f3a3259cd)

That is a different rule that is tracked by #1198. The title of this issue is a bit confusing. `--srx` sorts the strings inside of `__all__`, while this request is about moving the `__all__` (and `__version__`, `__author__`, etc.) _assignment statements_ to the top of the file, e.g.

Bad:
```py
"""Dummy module"""

from __future__ import annotations

import os

__all__ = ["foo"]
```

Good:
```py
"""Dummy module"""

from __future__ import annotations

__all__ = ["foo"]

import os
```

@nstarman , I think the title of this issue should be changed to something like "Add option to move module level dunders to top of file"

> Does that open sort the _members_ of `__all__`, or move the `__all__` itself? I couldn't tell from the PR.

The former.

---

_Comment by @alchzh on 2023-12-31 17:08_

It's worth noting that before 2016 the PEP8 mandated the opposite,
> Put any relevant ``__all__`` specification after the imports.

This was changed to the current text in
https://bugs.python.org/issue27187 ([patch](https://bugs.python.org/file43132/issue-27187-patch1.txt))
https://github.com/python/peps/commit/0aa70aebf102d352b8476f04509a369bf3db276c

but the style checkers only relaxed the previous rule instead of implementing the new one https://github.com/PyCQA/pycodestyle/pull/523 https://github.com/PyCQA/pycodestyle/issues/615 , so there's been no pressure to change it in existing codebases. It seems like most Python code follows the older convention still, since people tend to go by whatever their style checker / formatter says instead of reading PEP8 themselves.

I don't know if there's any other PEP8 suggestion that so wildly differs from what's done in practice.

---

_Renamed from "Add option to sort `__all__` according to PEP8" to "Add option to move module level dunders to top of file" by @nstarman on 2023-12-31 17:25_

---
