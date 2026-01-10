---
number: 733
title: F821 Undefined name when using Literal from typing
type: issue
state: closed
author: adilkhash
labels:
  - bug
assignees: []
created_at: 2022-11-14T07:09:48Z
updated_at: 2024-09-24T13:13:14Z
url: https://github.com/astral-sh/ruff/issues/733
synced_at: 2026-01-10T01:22:38Z
---

# F821 Undefined name when using Literal from typing

---

_Issue opened by @adilkhash on 2022-11-14 07:09_

Hey 👋,

I discovered the problem with Literal discussed in another issue: https://github.com/charliermarsh/ruff/issues/629 but I have a slightly different use case which `ruff` doesn't like:

```python
import typing as t


class MyClass:
    category: t.Literal['A', 'B', 'C']
````

```bash
> ruff --version
ruff 0.0.117

> ruff mycode.py
Found 3 error(s).
mycode.py:5:25: F821 Undefined name `A`
mycode.py:5:30: F821 Undefined name `B`
mycode.py:5:35: F821 Undefined name `C`
```

It is interesting that if I import Literal from typing without using an alias then everything is OK:

```python
from typing import Literal


class MyClass:
    category: Literal['A', 'B', 'C']
````

---

_Comment by @charliermarsh on 2022-11-14 14:32_

Yeah this is a legitimate deficiency right now! It’s similar to #720 but a slightly different case. We don’t track import aliases right now. But what you have here feels like a common pattern so I’ll try to prioritize fixing it.

---

_Label `bug` added by @charliermarsh on 2022-11-14 14:32_

---

_Comment by @charliermarsh on 2022-11-14 15:37_

(I'll try to fix this today.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-14 16:01_

---

_Referenced in [astral-sh/ruff#746](../../astral-sh/ruff/pulls/746.md) on 2022-11-15 01:21_

---

_Closed by @charliermarsh on 2022-11-15 02:29_

---

_Comment by @adilkhash on 2022-11-15 03:33_

Thanks for your quick responses, but I see that you've released the 0.0.119 version, but it doesn't solve the problem:

```bash
> ruff --version           
ruff 0.0.119

> ruff mycode.py
Found 3 error(s).
mycode.py:5:25: F821 Undefined name `A`
mycode.py:5:30: F821 Undefined name `B`
mycode.py:5:35: F821 Undefined name `C`


---

_Comment by @charliermarsh on 2022-11-15 03:50_

🤦 I missed a one-line change specific to `Literal`, one sec.

---

_Referenced in [astral-sh/ruff#748](../../astral-sh/ruff/pulls/748.md) on 2022-11-15 03:53_

---

_Comment by @charliermarsh on 2022-11-15 03:54_

Cutting a new release to fix this: [v0.0.120](https://github.com/charliermarsh/ruff/releases/tag/v0.0.120)

---

_Comment by @crypdick on 2024-02-15 15:40_

This issue appears to have resurfaced. Unlike the OP, the second example is not working for me:

```python
from typing import Literal
# same issue with`from beartype.typing import Literal`

class MyClass:
    category: Literal["A", "B", "C"]
```

```
> ruff --version
ruff 0.2.1
> ruff tmp.py
tmp.py:5:24: F821 Undefined name `A`
tmp.py:5:29: F821 Undefined name `B`
tmp.py:5:34: F821 Undefined name `C`
Found 3 errors.
```

---

_Comment by @crypdick on 2024-09-24 13:13_

@charliermarsh FYI this is still an issue on ruff 0.6.7
![image](https://github.com/user-attachments/assets/bfca0069-83b6-4073-b9f1-dff6c61626d3)


---
