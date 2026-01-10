```yaml
number: 3821
title: "`TCH004` false positive with submodule imports and rebinding"
type: issue
state: open
author: shiftinv
labels:
  - bug
assignees: []
created_at: 2023-03-30T18:02:07Z
updated_at: 2023-10-23T07:39:56Z
url: https://github.com/astral-sh/ruff/issues/3821
synced_at: 2026-01-10T11:09:46Z
```

# `TCH004` false positive with submodule imports and rebinding

---

_Issue opened by @shiftinv on 2023-03-30 18:02_

Importing the same top-level module through two submodule imports, one at runtime and one in a type-checking block, results in `TCH004` false positives.
This is certainly more of an edge case, seems like it could be related to #60 :thinking: 

Code:
```py
from __future__ import annotations

from typing import TYPE_CHECKING
import importlib.util

if TYPE_CHECKING:
    import importlib.machinery


def from_spec(spec: importlib.machinery.ModuleSpec):
    return importlib.util.module_from_spec(spec)
```

Running `ruff check --isolated --select TCH test.py` (v0.0.260) produces the following error:
```py
test.py:7:12: TCH004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
```

I don't know a whole lot about ruff's internals, but my assumption is that the `importlib.machinery` import in the type-checking block overwrites the `importlib` binding, and the `importlib.util.module_from_spec` call later resolves to that.

---

_Comment by @charliermarsh on 2023-03-30 18:12_

Hmm, what's happening here is that both submodule imports resolve to `importlib`, since submodule imports can resolve to an import of the parent -- e.g., this works:

```py
>>> import urllib.request
>>> urllib.response
<module 'urllib.response' from '/usr/local/Cellar/python@3.11/3.11.0/Frameworks/Python.framework/Versions/3.11/lib/python3.11/urllib/response.py'>
```

(I'm probably getting a bunch of terminology wrong, but in short, like Pyflakes, we tend to treat `import importlib.util` as identical to `import importlib`, which seems to be incorrect here.)


---

_Label `bug` added by @charliermarsh on 2023-03-30 18:47_

---

_Comment by @charliermarsh on 2023-03-30 18:47_

I'm not 100% sure how to fix in a general way but I agree that it's a bug here.

---

_Comment by @shiftinv on 2023-03-30 20:22_

Yeah, it's a bit of a weird bug. Treating `import a.b.c` like `import a` is completely fair for a linter, imo - with possible import side effects it's probably better to be  lenient with these things.

The original issue can be solved easily by just moving the `importlib.machinery` import, though I suppose at that point one could argue that it should emit a `TCH003` (`Move standard library import {} into a type-checking block`).

In the end it depends on how "using an import" is defined (e.g. does `a.b.func()` use `a.b` or `a`?) - at least in the general case, the current behavior is a good compromise, now that I think about it.
The alternative, which is being more strict about submodule imports, might end up creating a lot more different false positives, so... not sure.

---

_Comment by @Avasam on 2023-06-11 19:40_

Similar issue here with Python 3.9 and opencv-python, which just added a `typing` module (not released to pypi yet). But since it's not picked up by PyInstaller (yet), I wrapped the import in a `TYPE_CHECKING` check:

```py
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import cv2.typing

def is_blank(image: cv2.typing.MatLike):
    # do stuff here
    pass
```

Note that at runtime (normal python runtime outside of PyInstaller), the typing module exists, but is empty (https://github.com/opencv/opencv/issues/23782). So I know that this import is only used as type-hint otherwise `cv2.typing.MatLike` would fail.

---

_Comment by @dimbleby on 2023-08-29 13:06_

Another example that shows up pretty often is with types from `collections.abc` eg:
```python
  from typing import TYPE_CHECKING

  if TYPE_CHECKING:
     from collections.abc import Iterable

  def fun() -> Iterable[int]:
      yield from range(5)
```
```
foo.py:4:33: TCH004 [*] Move import `collections.abc.Iterable` out of type-checking block. Import is used for more than type hinting.`
```
which is clearly not right

---

_Comment by @charliermarsh on 2023-08-29 13:16_

@dimbleby - I believe that actually _is_ correct -- if you try running that code, you will get:

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/ruff/foo.py", line 6, in <module>
    def fun() -> Iterable[int]:
                 ^^^^^^^^
NameError: name 'Iterable' is not defined
```

Python evaluates types in function annotations at runtime as they're added to `__annotations__`. If you add `from __future__ import annotations`, Ruff will do the right thing. In a future version, we may add the ability to automatically quote those annotations (`"Iterable[int]"`) as part of the rule.

---
