```yaml
number: 8084
title: "`D102` & `D103` should not warn on functions where all overloads have docstrings"
type: issue
state: open
author: DetachHead
labels:
  - docstring
assignees: []
created_at: 2023-10-20T02:08:14Z
updated_at: 2023-10-22T11:14:46Z
url: https://github.com/astral-sh/ruff/issues/8084
synced_at: 2026-01-10T11:09:50Z
```

# `D102` & `D103` should not warn on functions where all overloads have docstrings

---

_Issue opened by @DetachHead on 2023-10-20 02:08_

```py
from typing import overload

@overload
def foo(a: int) -> None:
    """asdf"""

@overload
def foo(a: str) -> None:
    """fdsa"""

def foo(a: object) -> None: # D103: Missing docstring in public function
    ...
```
https://play.ruff.rs/20d01228-43dd-4364-a17d-c2a936781942

in vscode, the docstring that shows on hover is different depending on the overload being used, so i often use overload-specific documentation instead of documenting the implementation

---

_Label `docstring` added by @charliermarsh on 2023-10-20 21:19_

---

_Comment by @charliermarsh on 2023-10-20 21:21_

Hmm, this doesn't seem to be true in PyCharm, and I don't _think_ Sphinx supports it either. It seems like documenting the non-overloaded implementation is the best practice?

---

_Comment by @DetachHead on 2023-10-22 11:14_

perhaps it could be a setting?

---
