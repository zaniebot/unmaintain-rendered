```yaml
number: 16176
title: "TC003: Allow `TYPE_CHECKING = False`"
type: issue
state: closed
author: hugovk
labels: []
assignees: []
created_at: 2025-02-15T15:47:37Z
updated_at: 2025-02-15T18:12:20Z
url: https://github.com/astral-sh/ruff/issues/16176
synced_at: 2026-01-10T11:09:57Z
```

# TC003: Allow `TYPE_CHECKING = False`

---

_Issue opened by @hugovk on 2025-02-15 15:47_

### Description

TC003:

> Checks for standard library imports that are only used for type annotations, but aren't defined in a type-checking block.

https://docs.astral.sh/ruff/rules/typing-only-standard-library-import/#typing-only-standard-library-import-tc003

## `TYPE_CHECKING = False`

```python
from __future__ import annotations

TYPE_CHECKING = False
if TYPE_CHECKING:
    import os


def thing(locale: os.PathLike[str]) -> None: ...
```

Ruff doesn't allow it:

```sh
❯ ruff --version
ruff 0.9.6
❯ ruff check --select TC --isolated 1.py
1.py:5:12: TC003 Move standard library import `os` into a type-checking block
  |
3 | TYPE_CHECKING = False
4 | if TYPE_CHECKING:
5 |     import os
  |            ^^ TC003
  |
  = help: Move into type-checking block

Found 1 error.
```

But upstream https://pypi.org/project/flake8-type-checking/ does:
```
❯ flake8 --version
7.1.1 (flake8-2020: 1.8.1, flake8-implicit-str-concat: 0.5.0, flake8-type-checking: 3.0.0, mccabe: 0.7.0, pycodestyle: 2.12.1, pyflakes: 3.2.0) CPython 3.13.2 on Darwin
❯ flake8 1.py
❯ 
```


## `from typing import TYPE_CHECKING`

Both are fine with this:

```python
from __future__ import annotations

from typing import TYPE_CHECKING
if TYPE_CHECKING:
    import os
```
```sh
❯ ruff check --select TC --isolated 1.py
All checks passed!

❯ flake8 1.py --statistics
❯ 
```

But the whole point of this `typing-only-standard-library-import` TC003 check is to avoid imports that are only needed for type checking:

> Unused imports add a performance overhead at runtime, and risk creating import cycles. If an import is only used in typing-only contexts, it can instead be imported conditionally under an `if TYPE_CHECKING:` block to minimize runtime overhead.

See also:

* https://github.com/astral-sh/ruff/issues/15681
* https://github.com/astral-sh/ruff/pull/15719

Which look like it should have fixed it in 0.9.5, but I can repro in 0.9.4-0.9.6.

---

_Comment by @AlexWaygood on 2025-02-15 18:06_

Does this still repro if you use `--preview`? #15719 only changed the behaviour in preview mode for now, since it's technically a breaking change. We might be able to stabilise it in v0.10; if not, it'll probably be stabilised in v0.11 :-)

---

_Comment by @hugovk on 2025-02-15 18:11_

Hooray, it passes!

```console
❯ ruff --version && ruff check --select TC --isolated 1.py --preview
ruff 0.9.6
All checks passed!
```
Thanks!

---

_Closed by @hugovk on 2025-02-15 18:11_

---

_Comment by @AlexWaygood on 2025-02-15 18:12_

no worries!

---
