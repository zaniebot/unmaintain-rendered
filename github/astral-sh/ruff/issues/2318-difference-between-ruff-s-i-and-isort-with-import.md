---
number: 2318
title: "Difference between Ruff's I and isort with import *"
type: issue
state: closed
author: gvangool
labels:
  - bug
  - isort
assignees: []
created_at: 2023-01-29T02:37:37Z
updated_at: 2023-01-29T18:08:21Z
url: https://github.com/astral-sh/ruff/issues/2318
synced_at: 2026-01-07T13:12:14-06:00
---

# Difference between Ruff's I and isort with import *

---

_Issue opened by @gvangool on 2023-01-29 02:37_

There's a difference in the import ordering between isort and ruff when an `import *` is included.

A small test case:
```python
from .logging import config_logging
from .settings import *  # noqa F403
from .settings import ENV
```

When running both isort and ruff:
```
# isort --check --profile=black bug.py && echo "all good"
all good
# ruff check --select I001 --isolated bug.py
bug.py:1:1: I001 Import block is un-sorted or un-formatted
Found 1 error.
1 potentially fixable with the --fix option.
# ruff check --select I001 --isolated bug.py --fix
Found 1 error (1 fixed, 0 remaining).
# isort --check --profile=black bug.py
ERROR: ./bug.py Imports are incorrectly sorted and/or formatted.
```

The modified version of the test looks like
```python
from .logging import config_logging
from .settings import ENV
from .settings import *  # noqa F403
```

Relevant versions:
```
# ruff --version
ruff 0.0.237
# isort --version

                 _                 _
                (_) ___  ___  _ __| |_
                | |/ _/ / _ \/ '__  _/
                | |\__ \/\_\/| |  | |_
                |_|\___/\___/\_/   \_/

      isort your imports, so you don't have to.

                    VERSION 5.11.4

```

---

_Renamed from "Difference in isort with import *" to "Difference between Ruff's I and isort with import *" by @gvangool on 2023-01-29 02:38_

---

_Label `isort` added by @charliermarsh on 2023-01-29 02:59_

---

_Comment by @charliermarsh on 2023-01-29 03:02_

Oh interesting! Thank you.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-29 03:02_

---

_Label `bug` added by @charliermarsh on 2023-01-29 03:04_

---

_Referenced in [astral-sh/ruff#2320](../../astral-sh/ruff/pulls/2320.md) on 2023-01-29 03:14_

---

_Closed by @charliermarsh on 2023-01-29 03:17_

---

_Comment by @charliermarsh on 2023-01-29 03:18_

Should be fixed on `main`, will go out in the next release (tomorrow or Monday).

---

_Comment by @gvangool on 2023-01-29 04:46_

Thank you for that quick Saturday night support!

I can confirm it works with the latest `main`.

---

_Comment by @charliermarsh on 2023-01-29 05:56_

No problem! I work weird hours 😅 Thank you for confirming? (Just curious: did you build from source with `cargo`, or via `maturin`, or some other way to test on `main`?)

---

_Comment by @gvangool on 2023-01-29 18:07_

Pip and a local rust buildchain:
```
# python -m pip install -e 'git+https://github.com/charliermarsh/ruff#egg=ruff'
Obtaining ruff from git+https://github.com/charliermarsh/ruff#egg=ruff
  Cloning https://github.com/charliermarsh/ruff to ./.venv/src/ruff
  Running command git clone --filter=blob:none --quiet https://github.com/charliermarsh/ruff ./.venv/src/ruff
  Resolved https://github.com/charliermarsh/ruff to commit e371ef9b1a0f8e95776dea478cebf23b7517bf40
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... -
done
Building wheels for collected packages: ruff
  Building editable for ruff (pyproject.toml) ... done
  Created wheel for ruff: filename=ruff-0.0.237-py3-none-macosx_10_7_x86_64.whl size=4350535 sha256=e7e045c479a91388c2a6887d6abd94ed32c84de43135f8a4262f0dc230a732c7
  Stored in directory: /private/var/folders/fn/v91b3c2x0dx92kh4170ly38h0000gn/T/pip-ephem-wheel-cache-rs5r8ug2/wheels/89/5f/c5/3a1babdf0256db6d4dfc2d822a49320c2ba6f1d40fa4467b55
Successfully built ruff
Installing collected packages: ruff
Successfully installed ruff-0.0.237
```

---

_Comment by @charliermarsh on 2023-01-29 18:08_

Nice :)

---
