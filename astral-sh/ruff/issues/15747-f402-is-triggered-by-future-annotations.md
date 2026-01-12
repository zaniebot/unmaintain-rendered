```yaml
number: 15747
title: "F402 is triggered by `__future__.annotations`"
type: issue
state: closed
author: mattmess1221
labels:
  - question
assignees: []
created_at: 2025-01-26T02:25:32Z
updated_at: 2025-02-02T11:03:02Z
url: https://github.com/astral-sh/ruff/issues/15747
synced_at: 2026-01-12T15:54:54Z
```

# F402 is triggered by `__future__.annotations`

---

_@mattmess1221_

### Description

F402 is being triggered by `__future__` imports. These imports can probably be safely ignored.

Searched terms: F402, annotations, future

### Reproduction

```py
from __future__ import annotations

for annotations in [["anno1", "anno2"]]:
    print(annotations)
```

Output:

```console
% ruff --version
ruff 0.9.3
% ruff check --select F
f.py:3:5: F402 Import `annotations` from line 1 shadowed by loop variable
  |
1 | from __future__ import annotations
2 |
3 | for annotations in [["anno1", "anno2"]]:
  |     ^^^^^^^^^^^ F402
4 |     print(annotations)
  |

Found 1 error.
```

---

_Comment by @InSyncWithFoo on 2025-01-26 03:59_

This seems to be by design, considering what Pyflakes does:

```python
# https://github.com/PyCQA/pyflakes/blob/d9e32c4c/pyflakes/checker.py#L971
if isinstance(existing, Importation) and isinstance(parent_stmt, FOR_TYPES):
    self.report(messages.ImportShadowedByLoopVar,
                node, value.name, existing.source)

# https://github.com/PyCQA/pyflakes/blob/d9e32c4c/pyflakes/checker.py#L415
class FutureImportation(ImportationFrom):
    """
    A binding created by a from `__future__` import statement.

    `__future__` imports are implicitly used.
    """
    ...
```


---

_Comment by @AlexWaygood on 2025-01-26 10:24_

Yes, putting `from __future__ import annotations` in a module does actually insert an `annotations` symbol into the module's namespace:

```pycon
>>> from __future__ import annotations
>>> annotations
_Feature((3, 7, 0, 'beta', 1), None, 16777216)
```

This is documented at <https://docs.python.org/3/library/__future__.html>.

I appreciate that most people don't ever use the symbol that's inserted as a result of these `import` statements -- the main use case is the "side effect" the import statement has that causes the module to be parsed in a different way. However, your code still might be more readable if you used a different variable name to differentiate your loop variable from the symbol inserted by the import statement.

---

_Label `question` added by @AlexWaygood on 2025-01-26 10:24_

---

_Closed by @MichaReiser on 2025-02-02 11:03_

---
