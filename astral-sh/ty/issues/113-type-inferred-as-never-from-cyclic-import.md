```yaml
number: 113
title: "Type inferred as `Never` from cyclic * import"
type: issue
state: open
author: MatthewMckee4
labels:
  - bug
  - imports
assignees: []
created_at: 2025-04-23T20:44:53Z
updated_at: 2025-11-13T06:28:34Z
url: https://github.com/astral-sh/ty/issues/113
synced_at: 2026-01-10T02:06:24Z
```

# Type inferred as `Never` from cyclic * import

---

_Issue opened by @MatthewMckee4 on 2025-04-23 20:44_

### Summary

My reproduction is maybe not great, but there is a package `klayout` that contains a `.pyi` file with > 75,000 lines.
```bash
mkdir /tmp/test-repro
cd /tmp/test-repro
uv venv
uv pip install klayout
echo "import klayout.db as kdb

from typing_extensions import reveal_type


def _(x: kdb.LayerInfo, y: kdb.LEFDEFReaderConfiguration):
    reveal_type(x)
    reveal_type(y)
" > main.py
uv run <red-knot-bin> check main.py
```

This gives the following error:
```bash
error: lint:invalid-type-form: Variable of type `Never` is not allowed in a type expression
 --> /tmp/test-repro/main.py:6:10
  |
6 | def _(x: kdb.LayerInfo):
  |          ^^^^^^^^^^^^^
7 |     reveal_type(x)
  |

info: revealed-type: Revealed type
 --> /tmp/test-repro/main.py:7:5
  |
6 | def _(x: kdb.LayerInfo):
7 |     reveal_type(x)
  |     ^^^^^^^^^^^^^^ `Unknown`
  |

Found 2 diagnostics
```

Also to note `LayerInfo` is defined at line 36702 and `LEFDEFReaderConfiguration` at line 35628 (and `LEFDEFReaderConfiguration` works fine)

---

_Comment by @carljm on 2025-04-23 22:01_

Here's a minimal repro:

`main.py`:
```py
from pkg.sub import A

reveal_type(A)  # reveals Never
```

`pkg/outer.py`:
```py
class A: ...
```

`pkg/sub/__init__.py`:
```py
from ..outer import *
from .inner import *
```

`pkg/sub/inner.py`:
```py
from pkg.sub import A
```

---

_Label `bug` added by @carljm on 2025-04-23 22:01_

---

_Comment by @MatthewMckee4 on 2025-04-23 22:02_

Thank you!

---

_Comment by @MatthewMckee4 on 2025-04-23 22:06_

Also, my comment about the lines doesn't matter

---

_Comment by @MatthewMckee4 on 2025-04-23 22:09_

Also, this works (as in it fails) for a mdtest

## Relative import to `.pyi`

```toml
[environment]
python = "/src/.venv"
python-version = "3.13"
```

`/src/.venv/<path-to-site-packages>/package/sub/__init__.py`:

```py
from ..bar import *
from .inner import *
```

`/src/.venv/<path-to-site-packages>/package/sub/inner.py`:

```py
from package.sub import A
```

`/src/.venv/<path-to-site-packages>/package/bar.pyi`:

```pyi
class A: ...
```

`/src/main.py`:

```py
import package.sub as sub

reveal_type(sub.A)  # revealed: Literal[A]
```


---

_Comment by @MatthewMckee4 on 2025-04-23 22:12_

And, Importantly, this passes
## Relative import to `.pyi`

```toml
[environment]
python = "/src/.venv"
python-version = "3.13"
```

`/src/.venv/<path-to-site-packages>/package/sub/__init__.py`:

```py
from ..bar import *
reveal_type(A)  # revealed: Literal[A]
from .inner import *
reveal_type(A)  # revealed: Never
```

`/src/.venv/<path-to-site-packages>/package/sub/inner.py`:

```py
from package.sub import A
```

`/src/.venv/<path-to-site-packages>/package/bar.pyi`:

```pyi
class A: ...
```

`/src/main.py`:

```py
import package.sub as sub

reveal_type(sub.A)  # revealed: Never
```


---

_Renamed from "[red-knot] Types inferred as `Never` from large `.pyi` files." to "[red-knot] Type inferred as `Never` from cyclic * import" by @carljm on 2025-04-23 22:20_

---

_Comment by @Skylion007 on 2025-05-06 15:03_

This is a massive issue in torch as it prevents `from torch import Tensor` from working with ty.

---

_Renamed from "[red-knot] Type inferred as `Never` from cyclic * import" to "Type inferred as `Never` from cyclic * import" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @carljm on 2025-05-07 16:54_

> This is a massive issue in torch as it prevents `from torch import Tensor` from working with ty.

Although the symptom is basically identical, that's actually a different issue, and it's fixed now with the merge of https://github.com/astral-sh/ruff/pull/17856

The issue described above still repros, however; it seems less common.

---

_Label `imports` added by @AlexWaygood on 2025-05-10 18:05_

---

_Comment by @MatthewMckee4 on 2025-05-15 22:01_

I think this is resolved? my repro works fine now, and on my codebase i dont have the issue anymore

---

_Comment by @carljm on 2025-05-16 02:20_

I still see the type of `A` wrongly revealed as `Never` in the MRE I showed in https://github.com/astral-sh/ty/issues/113#issuecomment-2859014192 so I think there is still something to fix here.

---

_Comment by @MatthewMckee4 on 2025-05-17 16:18_

Ah i see, my mistake. I wonder what the difference is between our repros then.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:28_

---
