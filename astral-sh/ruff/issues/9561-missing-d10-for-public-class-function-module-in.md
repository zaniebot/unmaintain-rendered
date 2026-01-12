```yaml
number: 9561
title: "Missing D10* for \"public\" class / function / module in \"private\" module"
type: issue
state: open
author: szleb
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-01-17T09:35:21Z
updated_at: 2024-10-24T14:33:02Z
url: https://github.com/astral-sh/ruff/issues/9561
synced_at: 2026-01-12T15:54:49Z
```

# Missing D10* for "public" class / function / module in "private" module

---

_@szleb_

Hi there,

imagine you have the following folder with files
package
├── \_\_init\_\_.py
└── \_private.py

Content of \_private.py:
```
class PublicClass:
    pass
```

Content of \_\_init\_\_.py:
```
from ._private import PublicClass

__all__ = ["PublicClass"]
```

If you now run
`ruff package --select D`
the output is
`package\__init__.py:1:1: D104 Missing docstring in public package`

I miss the error
`package\_private.py:1:7: D101 Missing docstring in public class`

Running pydocstyle does show D101 but unfortunately no matter whether \_\_init\_\_.py does export PublicClass or not.

In my opinion the optimal behavior would be to raise D101 when PublicClass is exported in \_\_init\_\_.py and to NOT raise the error when PublicClass is not exported in a public package (or module) but hidden by the "private" module \_private.py. But I have no idea if this is feasible. If not, I would prefer the behavior of pydocstyle which seems just raise D10* for any function, module or class not starting with a single underline.

---

_Label `rule` added by @AlexWaygood on 2024-01-17 13:16_

---

_Label `multifile-analysis` added by @AlexWaygood on 2024-01-17 13:16_

---

_Comment by @MichaReiser on 2024-03-21 13:21_

This is an interesting case because it shows that there's a cycle:

* `__init__.py` depends on `_private.py` to know the type of `PublicClass`
* `PublicClass` (`_private.py`) depends on `__init__.py` to know that `PublicClass` is public. 

This is related to https://github.com/astral-sh/ruff/issues/7154

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
