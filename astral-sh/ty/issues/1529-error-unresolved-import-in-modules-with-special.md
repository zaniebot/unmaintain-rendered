```yaml
number: 1529
title: "error[unresolved-import] in modules with special names"
type: issue
state: closed
author: FaneTheEternal
labels:
  - bug
  - imports
assignees: []
created_at: 2025-11-12T08:15:47Z
updated_at: 2025-11-14T09:12:13Z
url: https://github.com/astral-sh/ty/issues/1529
synced_at: 2026-01-10T02:06:25Z
```

# error[unresolved-import] in modules with special names

---

_Issue opened by @FaneTheEternal on 2025-11-12 08:15_

### Summary

Sample structure
```
project/
├── mod1/
│   ├── __init__.py
│   ├── consts.py
│   └── def.py
└── main.py
```
In `mod1/def.py`
```python
from . import consts
```
On `uvx ty check`
```
error[unresolved-import]: Cannot resolve imported module `.`
 --> mod1\def.py:1:1
  |
1 | from . import consts
  | ^^^^^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-import` is enabled by default
```
If rename `def.py` to `define.py` the error will disappear

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Label `imports` added by @sharkdp on 2025-11-12 08:22_

---

_Comment by @MichaReiser on 2025-11-12 10:47_

I think this is because we enforce that each segment passed to `ModuleName::new` is a valid module name and `def` isn't a valid module name. 

However, `def` being a valid module name is only important when trying to import it, not when trying to import another module from `def.py`. 

I remember having seen a similar issue where relative imports fail if any folder between the python file and the first search root contains an invalid name. E.g. if you have `web/backend-core/module.py` and the src root is `/`, any relative import within `web/backend-core` fails because `backend-core` isn't a valid module name

---

_Comment by @FaneTheEternal on 2025-11-12 11:09_

It seems that this is purely a syntactic limitation.
`main.py`
```python
import ast

try:
    eval("from mod1 import def", globals())
except SyntaxError as e:
    pass
assert "def" not in globals()

import_def = ast.Module(
    body=[
        ast.ImportFrom(
            module="mod1",
            names=[
                ast.alias(
                    name="def",
                    lineno=0,
                    col_offset=0,
                ),
            ],
            level=0,
            lineno=0,
            col_offset=0,
        )
    ],
    type_ignores=[],
)
eval(compile(import_def, "<ast>", mode="exec"), globals())
assert "def" in globals()
```
P.s. In our django project, we import the modules of the same name from each application using `importlib.import_module`

---

_Label `bug` added by @MichaReiser on 2025-11-12 11:43_

---

_Comment by @carljm on 2025-11-12 15:05_

> However, `def` being a valid module name is only important when trying to import it, not when trying to import another module from `def.py`.

This isn't (currently) true for our implementation of relative imports, where the first step is "resolve my own module name" so then we can resolve the relative import relative to it.
 
That's the same reason it would fail if there's an invalid module name in the path from search root to current file.

Maybe we need a less-strict version of "resolve my own module name" to use in this case?

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:12_

---

_Closed by @MichaReiser on 2025-11-14 09:12_

---
