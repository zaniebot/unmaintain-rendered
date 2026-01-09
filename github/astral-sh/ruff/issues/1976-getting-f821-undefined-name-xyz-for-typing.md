---
number: 1976
title: "Getting F821 Undefined name \"xyz\" for typing.Literal values when imported indirectly"
type: issue
state: closed
author: alisaifee
labels: []
assignees: []
created_at: 2023-01-18T22:59:45Z
updated_at: 2023-02-07T02:31:55Z
url: https://github.com/astral-sh/ruff/issues/1976
synced_at: 2026-01-07T13:12:14-06:00
---

# Getting F821 Undefined name "xyz" for typing.Literal values when imported indirectly

---

_Issue opened by @alisaifee on 2023-01-18 22:59_

# Description

Ruff complains with `F821 Undefined name "xyz"` for the values described in arguments typed as `typing.Literal`. I was however only able to reproduce this when `Literal` is imported indirectly (i.e. not `from typing import Literal`).

As an example, the following file passes all checks:

```py
from typing import Literal

a: Literal["fu", "bar"] = "fu"
a = "bar"
```

But this does not:

- x.py
  ```py
  from typing import Literal
  __all__ = ["Literal"]
  ```

- y.py
  ```py
  from x import Literal
  a: Literal["fu", "bar"] = "fu"
  a = "bar"
  ```
- Output:
  ```
  $ ruff y.py
  y.py:2:12: F821 Undefined name `fu`
  y.py:2:18: F821 Undefined name `bar`
  ```

- Reproduced without any configuration changes.
- Ruff version:
  ```
  $ ruff --version
  ruff 0.0.225
  ```

---

_Renamed from "Getting F821 Undefined name "xyz" for typing.Literal " to "Getting F821 Undefined name "xyz" for typing.Literal values when imported indirectly" by @alisaifee on 2023-01-18 23:03_

---

_Comment by @charliermarsh on 2023-01-18 23:03_

Yeah we don't track that the re-exported `Literal` in that case is the same as `typing.Literal`. 

We do have something to support this use-case though. If you want to treat every member of `x` as if it's a re-export of a `typing` member, you can do:

```toml
[tool.ruff]
typing-modules = ["x"]
```

(Relevant docs are here: https://github.com/charliermarsh/ruff#typing-modules)


---

_Comment by @charliermarsh on 2023-01-18 23:04_

(The canonical use-case for this is projects that have some kind of compatibility shim over `typing` and `typing-extensions`.)

---

_Comment by @alisaifee on 2023-01-18 23:17_

Neat, that is exactly the use case ([reference](https://github.com/alisaifee/coredis/blob/master/coredis/typing.py)).



---

_Comment by @alisaifee on 2023-01-18 23:18_

I'll close this as it is clearly pointed out in the documentation (😅)

---

_Closed by @alisaifee on 2023-01-18 23:18_

---

_Comment by @charliermarsh on 2023-01-18 23:19_

Oh nice! Good to know that there are more examples of this, and glad it solves your problem :)


---

_Comment by @henryiii on 2023-01-19 22:16_

Solves my problem too, thank you! Though I use relative imports, so it seems to not be triggering on `from .._compat.typing import Literal`. If I change it to absolute, then it works.

---

_Referenced in [astral-sh/ruff#2005](../../astral-sh/ruff/issues/2005.md) on 2023-01-19 22:28_

---

_Comment by @charliermarsh on 2023-01-19 22:28_

Ah good call -- filed as #2005!

---

_Comment by @charliermarsh on 2023-02-07 02:31_

@henryiii - Heads up, I also checked out `scikit_build_core` and verified that (e.g.) `F821` errors aren't being triggered for Literals with `typing-modules = ["scikit_build_core._compat.typing"]`.

---
