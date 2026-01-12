```yaml
number: 15723
title: "[`flake8-type-checking`] False positives/negatives for TC001-004 for overlapping module imports"
type: issue
state: open
author: Daverball
labels:
  - bug
assignees: []
created_at: 2025-01-24T16:13:14Z
updated_at: 2025-12-04T06:59:08Z
url: https://github.com/astral-sh/ruff/issues/15723
synced_at: 2026-01-12T15:54:54Z
```

# [`flake8-type-checking`] False positives/negatives for TC001-004 for overlapping module imports

---

_@Daverball_

### Description

This bug was discovered in #15719

Given something like:

```python
from __future__ import annotations

import importlib.abc
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import importlib.machinery

class Foo(importlib.abc.MetaPathFinder):
    def bar(self) -> importlib.machinery.ModuleSpec: ...
```

`importlib.machinery` will incorrectly be flagged with `TC004`, since `importlib.abc.MetaPathFinder` binds to the same binding as `importlib.machinery.ModuleSpec`, so since the former is used at runtime, it thinks that `importlib.machinery` needs to be moved outside the `TYPE_CHECKING` block.

Combining the two imports into a shared binding makes sense, since what will be looked up is `importlib` and not `importlib.machinery` or `importlib.abc`.

This means that when we're checking the import bindings we need to expand the single `importlib` binding to its two import statements `importlib.abc` and `importlib.machinery` and for each reference check which of the two imports will actually be relevant.

---

_Label `bug` added by @MichaReiser on 2025-01-24 17:34_

---

_Comment by @coredumperror on 2025-12-04 00:13_

Could this possibly be why I'm getting `TC004` from the following code? 

```python
from typing import TYPE_CHECKING
from django.urls import reverse_lazy

if TYPE_CHECKING:
    from django_stubs_ext import StrOrPromise


class ApplicationTable:

    #: The URL to submit form actions to
    form_url: StrOrPromise = reverse_lazy("operations:applications--actions")
```

https://play.ruff.rs/d3e8ee0f-a7ba-477c-964c-d59bda9c396d

It doesn't seem to make any sense that this would be triggering TC004.

---

_Comment by @Daverball on 2025-12-04 06:59_

No, it makes perfect sense that this would emit `TC004`.

It's because your target version is Python 3.9 and you're not using `from __future__ import annotations`, so all annotations are evaluated at runtime, so this code will fail with a `NameError` at runtime if you run it with Python 3.9 - 3.13, it only works like this on Python 3.14.

Overlapping imports are only a concern if you import entire submodules from the same parent package.

---
