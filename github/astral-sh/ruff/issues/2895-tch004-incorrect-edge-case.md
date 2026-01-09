---
number: 2895
title: TCH004 incorrect edge case
type: issue
state: closed
author: nstarman
labels:
  - bug
assignees: []
created_at: 2023-02-14T16:27:49Z
updated_at: 2023-02-14T16:43:50Z
url: https://github.com/astral-sh/ruff/issues/2895
synced_at: 2026-01-07T13:12:14-06:00
---

# TCH004 incorrect edge case

---

_Issue opened by @nstarman on 2023-02-14 16:27_

I found an incorrect detection of ``TCH004``. MWE:

```
from typing import Any, TYPE_CHECKING

if TYPE_CHECKING:
    from numpy import floating
    from numpy.typing import NDArray
    from typing_extensions import TypeAlias

    NDFloating: TypeAlias = NDArray[floating[Any]]

def func(x: NDFloating) -> None:
    return
```

ruff is saying ``floating`` and ``NDArray`` should be moved out of the type-checking block because the import is used for more than type hinting. However ``NDFloating`` is constructed in the TYPE_CHECKING block, so I think this is a false positive.

---

_Comment by @charliermarsh on 2023-02-14 16:43_

I _think_ this is fixed in the next release.

---

_Comment by @charliermarsh on 2023-02-14 16:43_

I.e., I _think_ this is the same as #2852.

---

_Closed by @charliermarsh on 2023-02-14 16:43_

---

_Label `bug` added by @charliermarsh on 2023-02-14 16:43_

---
