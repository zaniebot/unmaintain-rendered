---
number: 2104
title: "Potential inconsistency between Ruff's ``I`` and isort"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
  - isort
assignees: []
created_at: 2023-01-23T08:52:36Z
updated_at: 2024-01-10T21:09:54Z
url: https://github.com/astral-sh/ruff/issues/2104
synced_at: 2026-01-07T13:12:14-06:00
---

# Potential inconsistency between Ruff's ``I`` and isort

---

_Issue opened by @AA-Turner on 2023-01-23 08:52_

I believe this is distinct from https://github.com/charliermarsh/ruff/issues/1718; cross-linking in case there are similarities, though.

isort version: 5.11.4
Ruff version: 230

isort config:
```toml
[tool.isort]
line_length = 95
profile = "black"
remove_redundant_aliases = true
```

Commands:

```powershell
(sphinx) PS I:\Development\sphinx> ruff -V        
ruff 0.0.230
(sphinx) PS I:\Development\sphinx> type bug.py                                   
from operator import add  # NoQA: F401
from operator import mul, sub
(sphinx) PS I:\Development\sphinx> ruff --isolated --select I bug.py
bug.py:1:1: I001 Import block is un-sorted or un-formatted
Found 1 error(s).
1 potentially fixable with the --fix option.
(sphinx) PS I:\Development\sphinx> isort --check bug.py 
```

Note Ruff wants to re-format to:

```python
from operator import (
    add,  # NoQA: F401
    mul,
    sub,
)

```

whereas ``isort`` wants to reformat to:
```python
from operator import add  # NoQA: F401
from operator import mul, sub
```

This appeared in the Sphinx project where some imports are type-comment only (e.g. in giving type annotations to iterators, where the pattern is ``for item in items:  # type: blah`, should MyPy not be able to deduce the type of ``item``).

A

---

_Comment by @charliermarsh on 2023-01-23 17:35_

This does look like an incompatibility. I think the thing to decide is whether it's a known and accepted deviation, or not :)

Do you have a preference between the outputs? Did it cause any problems, or the issue is more that it deviated at all?

---

_Label `question` added by @charliermarsh on 2023-01-23 17:36_

---

_Label `isort` added by @charliermarsh on 2023-01-23 17:36_

---

_Comment by @Jackenmen on 2023-01-23 18:35_

Personally, I like Ruff's variant more as it keeps imports from a specific module in one statement. I don't know if the `noqa` comment applies properly in both cases though.

---

_Comment by @charliermarsh on 2023-01-23 18:37_

Ruff will respect that noqa in both cases, but I don't think Flake8 respects the version that Ruff outputs.

---

_Comment by @AA-Turner on 2023-01-24 00:36_

> the issue is more that it deviated at all?

This is the case for me, we run both isort and Ruff in continuous integration testing so either will fail unless an ungainly `isort: skip` or ruff-specific NoQA comment were added.

I don't particularly mind which style 'wins'.

A

---

_Comment by @skshetry on 2023-01-29 11:22_

I prefer isort's output here as it is more granular for the ignores/disables to work. `pylint` for example cannot work with inlined-disables. Take this code for example, from a codebase that I work with:
```python
from dvc.api.data import open  # pylint: disable=redefined-builtin
from dvc.api.data import read

__all__ = ["read", "open"]
```

`ruff` changes it to:
```diff
--- script.py
+++ script.py
@@ -1,4 +1,6 @@
-from dvc.api.data import open  # pylint: disable=redefined-builtin
-from dvc.api.data import read
+from dvc.api.data import (
+    open,  # pylint: disable=redefined-builtin
+    read,
+)
 
 __all__ = ["read", "open"]
```

And, `pylint` now fails:
```console
script.py:1:0: W0622: Redefining built-in 'open' (redefined-builtin)
```

---

_Comment by @charliermarsh on 2023-01-29 17:24_

Yeah I think we should likely change this going forward given the handling of action comments.

---

_Label `question` removed by @charliermarsh on 2023-01-29 17:24_

---

_Label `bug` added by @charliermarsh on 2023-01-29 17:24_

---

_Referenced in [python-telegram-bot/python-telegram-bot#3594](../../python-telegram-bot/python-telegram-bot/pulls/3594.md) on 2023-03-19 18:07_

---

_Referenced in [microsoft/LightGBM#5871](../../microsoft/LightGBM/pulls/5871.md) on 2023-05-11 04:05_

---

_Comment by @andersk on 2023-08-02 22:04_

> `pylint` for example cannot work with inlined-disables.

Sure it can, you just need to move the comment to the right line.

```python
from dvc.api.data import (  # pylint: disable=redefined-builtin
    open,
    read,
)

__all__ = ["read", "open"]
```

Ruff can’t be sure in advance which line the comment belongs on (and honestly, its first guess makes more sense), but once you put it on the right line, Ruff will leave it there.

---

_Comment by @charliermarsh on 2024-01-10 21:09_

I'm going to close this for now as an intentional deviation. If we receive more reports or more feedback on it, as always, I'll reconsider.

---

_Closed by @charliermarsh on 2024-01-10 21:09_

---

_Referenced in [microsoft/LightGBM#6304](../../microsoft/LightGBM/issues/6304.md) on 2024-02-08 22:51_

---

_Referenced in [emlid/code-quality#24](../../emlid/code-quality/pulls/24.md) on 2025-02-24 14:36_

---
