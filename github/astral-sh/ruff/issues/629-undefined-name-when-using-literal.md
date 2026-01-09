---
number: 629
title: "Undefined name when using `Literal`"
type: issue
state: closed
author: matteosantama
labels: []
assignees: []
created_at: 2022-11-07T01:48:22Z
updated_at: 2022-11-07T02:09:51Z
url: https://github.com/astral-sh/ruff/issues/629
synced_at: 2026-01-07T13:12:14-06:00
---

# Undefined name when using `Literal`

---

_Issue opened by @matteosantama on 2022-11-07 01:48_

```
❯ ruff --version
ruff 0.0.104
```

I am getting `F821 Undefined name 'uniform'` on this line,

```python
pi: Literal["uniform"] | torch.Tensor = "uniform"
```

Perhaps it has to do with the way I am importing `Literal`,

```python
if sys.version_info >= (3, 8):
    from typing import Literal
else:
    from typing_extensions import Literal
```

---

_Comment by @matteosantama on 2022-11-07 01:50_

When I remove the `if/else` and import directly `from typing import Literal` there is no error. But when I instead to import directly with `from typing_extensions import Literal` the error crops up again.

So it is related to the usage of `typing_extensions`, not the `if/else`.

---

_Comment by @charliermarsh on 2022-11-07 01:51_

Ok cool. Both should work. Let me take a look.

---

_Comment by @charliermarsh on 2022-11-07 01:53_

This file runs for me without error:

```py
import sys

import torch


if sys.version_info >= (3, 8):
    from typing import Literal
else:
    from typing_extensions import Literal

pi: Literal["uniform"] | torch.Tensor = "uniform"
```

Can you help me tweak it to reproduce?


---

_Comment by @matteosantama on 2022-11-07 01:55_

Hm ok, let me add a few more things.

```python
from __future__ import annotations

import sys
from dataclasses import dataclass

import torch

if sys.version_info >= (3, 8):
    from typing import Literal
else:
    from typing_extensions import Literal


@dataclass
class FictitiousPlay:  # <-- in my code, this actually subclasses something

    pi: Literal["uniform"] | torch.Tensor = "uniform"
```

---

_Comment by @charliermarsh on 2022-11-07 01:56_

That's perfect, thanks.

---

_Comment by @charliermarsh on 2022-11-07 01:56_

(The `from __future__ import annotations` is necessary to reproduce.)

---

_Comment by @charliermarsh on 2022-11-07 01:56_

Will fix this.

---

_Referenced in [astral-sh/ruff#630](../../astral-sh/ruff/pulls/630.md) on 2022-11-07 02:03_

---

_Comment by @charliermarsh on 2022-11-07 02:03_

Thank you!

---

_Closed by @charliermarsh on 2022-11-07 02:03_

---

_Comment by @charliermarsh on 2022-11-07 02:03_

I'll cut another release in a little bit.

---

_Comment by @matteosantama on 2022-11-07 02:09_

Man you're speedy! Thanks!

---

_Referenced in [astral-sh/ruff#733](../../astral-sh/ruff/issues/733.md) on 2022-11-14 07:09_

---
