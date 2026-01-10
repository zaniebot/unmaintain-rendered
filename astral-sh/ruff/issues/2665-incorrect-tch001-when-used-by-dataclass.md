```yaml
number: 2665
title: Incorrect TCH001 when used by dataclass
type: issue
state: closed
author: nstarman
labels:
  - bug
  - typing
assignees: []
created_at: 2023-02-08T18:10:51Z
updated_at: 2023-02-08T21:03:18Z
url: https://github.com/astral-sh/ruff/issues/2665
synced_at: 2026-01-10T11:09:45Z
```

# Incorrect TCH001 when used by dataclass

---

_Issue opened by @nstarman on 2023-02-08 18:10_

In the following example ruff flags ``Bar`` for TCH001, but python requires `Bar` to be run-time accessible.

```python

from bar import Bar

@dataclass
class Foo:
    x: Bar
```

---

_Label `bug` added by @charliermarsh on 2023-02-08 18:17_

---

_Comment by @charliermarsh on 2023-02-08 18:24_

Oh hmmm... Maybe `AnnAssign` statements are always required to be runtime-evaluatable.

---

_Comment by @charliermarsh on 2023-02-08 18:24_

(The original plugin also flags this as an error.)

---

_Label `typing` added by @charliermarsh on 2023-02-08 18:24_

---

_Comment by @charliermarsh on 2023-02-08 18:28_

Wow, fascinating. The annotations are evaluated IFF in a module or class scope: https://docs.python.org/3/reference/simple_stmts.html#annotated-assignment-statements.

---

_Comment by @charliermarsh on 2023-02-08 18:35_

Very fun, will fix!

https://twitter.com/charliermarsh/status/1623389778375843840

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-08 18:52_

---

_Closed by @charliermarsh on 2023-02-08 18:59_

---

_Comment by @nstarman on 2023-02-08 20:12_

Thanks for the quick fix!

---

_Comment by @charliermarsh on 2023-02-08 20:24_

Are you able to link the file that this occurred on? Just so I can test before this release.

---

_Comment by @nstarman on 2023-02-08 21:01_

Ahh. Private repo. Let me make an example real quick... Ok this definitely tripped TCH001 and then broke the dataclass when I fixed it.

```python
from dataclasses import dataclass
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Mapping

@dataclass
class Test:
    arg: Mapping[str, str]
```

---

_Comment by @charliermarsh on 2023-02-08 21:03_

Ok cool! That now yields `TCH004`, telling you to take it out of the block. And if you take it out of the block, no errors are triggered, so I think we're good :) Thanks.

---
