```yaml
number: 6014
title: "re-export `from typing import Literal as L`: F821 Undefined name `bar`"
type: issue
state: open
author: Jasha10
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-07-23T10:55:09Z
updated_at: 2024-10-24T14:33:41Z
url: https://github.com/astral-sh/ruff/issues/6014
synced_at: 2026-01-10T11:09:48Z
```

# re-export `from typing import Literal as L`: F821 Undefined name `bar`

---

_Issue opened by @Jasha10 on 2023-07-23 10:55_

Hello,

If I import `typing.Literal` in `lib.py` then do `from lib import Literal` in `main.py`, ruff fails to treat the `Literal` type annotations properly.

```python
# lib.py
from typing import Literal

__all__ = ["Literal"]
```
```python
# main.py
from lib import Literal

foo: Literal["bar"] = "bar"
```
```bash
$ ruff check *.py
main.py:4:15: F821 Undefined name `bar`
Found 1 error.
$ ruff --version
ruff 0.0.280
```

### Expected behavior:

Ruff should recognize that `Literal["bar"]` is an annotation for a string literal and not flag the name `bar` as undefined.

---

_Comment by @charliermarsh on 2023-07-23 15:56_

For now, you'll need to mark `lib` in [`typing-modules`](https://beta.ruff.rs/docs/settings/#typing-modules), like:

```python
[tool.ruff]
typing-modules = ["lib"]
```

This tells Ruff to treat exports from `lib` as if they are re-exports from `typing`. Let me know if it doesn't work for whatever reason :)

---

_Closed by @charliermarsh on 2023-07-23 15:56_

---

_Comment by @Jasha10 on 2023-07-23 16:48_

Thanks for the tip, @charliermarsh.
This still does not cover my use-case which involves `from typing import Literal as L` and then re-export of `L`. Here's an updated repro:

```python
# lib.py
from typing import Literal as L
from typing import Literal

__all__ = ["L", "Literal"]
```
```python
# main.py
from lib import L, Literal

foo1: Literal["bar"] = "bar"
foo2: L["bar"] = "bar"
```
```toml
# pyproject.toml
[tool.ruff]
typing-modules = ["lib"]
```
```bash
$ ruff check *.py
main.py:5:10: F821 Undefined name `bar`
Found 1 error.
```

I'll update this issue's name accordingly.

---

_Renamed from "re-export `typing.Literal`: F821 Undefined name `bar`" to "re-export `from typing import Literal as L`: F821 Undefined name `bar`" by @Jasha10 on 2023-07-23 16:49_

---

_Reopened by @charliermarsh on 2023-07-23 17:29_

---

_Comment by @charliermarsh on 2023-07-23 17:30_

Makes sense, thanks. To set expectations, this won't be fixed in an imminent release since it requires us to resolve symbols across files, but it will be fixed eventually.

---

_Label `bug` added by @charliermarsh on 2023-07-24 02:22_

---

_Label `type-inference` added by @charliermarsh on 2023-07-24 02:22_

---

_Comment by @rlipperts on 2023-07-25 06:33_

I also stumbled upon this while evaluating if I want to use [nptyping](https://github.com/ramonhagenaars/nptyping) in my work project:

```python
from nptyping import Float, NDArray, Shape

my_array: NDArray[Shape["x, y, z, 3"], Float]
```

results in

```bash
$ ruff check nptyping_test.py
nptyping_test.py:3:26: F821 Undefined name `x`
nptyping_test.py:3:29: F821 Undefined name `y`
nptyping_test.py:3:32: F821 Undefined name `z`
Found 3 errors.
```

even with 

```toml
typing-modules = ["nptyping"]
```

in my `pyproject.toml`. 

As it seems this is a real-world example of the issue described by @Jasha10 - [nptyping uses Literal for their shape Keyword](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md?plain=1#L117C00-L126C82).


---

_Comment by @Jasha10 on 2023-07-25 17:10_

Thanks for adding this context @rlipperts. My usecase is also related to `ntyping` -- I'm using a personal fork of `ntyping` in my own project.

---

_Comment by @charliermarsh on 2023-07-25 17:14_

We could special-case `nptying.Shape` as an alias of `Literal`. (But it wouldn't solve cases in which that's re-exported under some other alias.)

---

_Comment by @rlipperts on 2023-07-25 17:21_

As outlined in the referenced link it is possible to replace `Shape` with `Literal`. For me that is a sufficient work-around so that there is no need to clutter your codebase, @charliermarsh,  if you intend to fix it in the long run.

---

_Comment by @Pierre-Sassoulas on 2023-07-26 10:02_

We also encounter something very similar in pylint when upgrading to 0.0.280, but on the actual ``typing.Litteral`` and not an alias:

```
pylint/utils/utils.py:56:31: F821 Undefined name `ignored`
pylint/utils/utils.py:56:39: F821 Undefined name `modules`
pylint/utils/utils.py:64:36: F821 Undefined name `py`
pylint/utils/utils.py:64:39: F821 Undefined name `version`
Found 4 errors.
```

When there are multiple values in the ``Literal`` it seems to works:
Code:

https://github.com/pylint-dev/pylint/blob/a57dd01c465a1357fc96833bec278fbf47147a4c/pylint/utils/utils.py#L27C1-L64 

---

_Comment by @charliermarsh on 2023-07-26 12:37_

@Pierre-Sassoulas - I believe that issue is separate and related to the use of future annotations. It’s fixed on main and will be out soon. (Similar to https://github.com/astral-sh/ruff/issues/6055.)

---

_Label `multifile-analysis` added by @AlexWaygood on 2024-03-12 16:06_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
