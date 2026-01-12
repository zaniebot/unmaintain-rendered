```yaml
number: 1718
title: "Discrepancies between ``isort`` and Ruff's ``I`` lint check"
type: issue
state: closed
author: AA-Turner
labels:
  - isort
assignees: []
created_at: 2023-01-07T18:47:41Z
updated_at: 2023-01-07T22:56:40Z
url: https://github.com/astral-sh/ruff/issues/1718
synced_at: 2026-01-12T15:54:41Z
```

# Discrepancies between ``isort`` and Ruff's ``I`` lint check

---

_@AA-Turner_

It seems Ruff expects a space after ``isort:skip`` (i.e. Ruff wants ``isort: skip``), and Ruff seems to ignore F401 for the purposes of import sorting, whereas ``isort`` honours them as skip markers(?).

Ruff also reports 1 error in the initial call, yet the ``--diff`` call reports three errors.

Ruff version 213; isort version 5.11.4

Sample file:

```powershell
(sphinx) PS I:\Development\sphinx> type bug.py            
from operator import add  # noqa: F401
from operator import sub  # noqa: F401

from math import pi  # noqa: F401  isort:skip
from math import e  # noqa: F401  isort:skip

from string import (  # noqa: E402  # isort:skip
    ascii_lowercase
)

from pprint import pprint  # isort:skip
(sphinx) PS I:\Development\sphinx> isort bug.py           
(sphinx) PS I:\Development\sphinx> ruff bug.py --select I
bug.py:1:1: I001 Import block is un-sorted or un-formatted
Found 1 error(s).
1 potentially fixable with the --fix option.
(sphinx) PS I:\Development\sphinx> ruff bug.py --diff    
--- bug.py
+++ bug.py
@@ -1,11 +1,8 @@
-from operator import add  # noqa: F401
-from operator import sub  # noqa: F401
-
-from math import pi  # noqa: F401  isort:skip
-from math import e  # noqa: F401  isort:skip
-
-from string import (  # noqa: E402  # isort:skip
-    ascii_lowercase
+from math import (
+    e,  # noqa: F401  isort:skip
+    pi,  # noqa: F401  isort:skip
+)
+from operator import (
+    add,  # noqa: F401
+    sub,  # noqa: F401
 )
-
-from pprint import pprint  # isort:skip

Would fix 3 error(s).
(sphinx) PS I:\Development\sphinx> ruff -V           
ruff 0.0.213
(sphinx) PS I:\Development\sphinx> 
```

A

---

_Label `isort` added by @charliermarsh on 2023-01-07 20:33_

---

_Comment by @charliermarsh on 2023-01-07 20:34_

Yeah this was sort of a known thing when I implemented it, but maybe that was a bad decision. (Both Ruff and isort should respect the version with the space, but Ruff doesn't respect the version without the space, IIUC.)

---

_Closed by @charliermarsh on 2023-01-07 22:30_

---

_Comment by @charliermarsh on 2023-01-07 22:30_

I fixed the `# isort:skip` part. Will look at the error count too.

---

_Comment by @AA-Turner on 2023-01-07 22:31_

Thanks for the quick fix!

A

---

_Comment by @charliermarsh on 2023-01-07 22:32_

When you ran `ruff bug.py --diff `, what other error codes were enabled? (Since there's no `--select I`.)

---

_Comment by @AA-Turner on 2023-01-07 22:43_

Ahh, sorry:

`tool.ruff.select`: 

```toml

select = [
    "E",   # pycodestyle
    "F",   # Pyflakes
    "W",   # pycodestyle
    # plugins:
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "PGH", # pygrep-hooks
    "PLC", # pylint
    "PLE", # pylint
    "PLR", # pylint
    "PLW", # pylint
    "S",   # flake8-bandit
    "SIM", # flake8-simplify
    "T10", # flake8-debugger
    "UP",  # pyupgrade
    "RUF100",  # yesqa
]
```

---

_Comment by @AA-Turner on 2023-01-07 22:49_


<details>
<summary>Full Ruff section:</summary>

```toml

[tool.ruff]
target-version = "py38"  # Pin Ruff to Python 3.8
line-length = 95
exclude = [
    '.git',
    '.tox',
    '.venv',
    'tests/roots/*',
    'build/*',
    'doc/_build/*',
    'sphinx/search/*',
    'doc/usage/extensions/example*.py',
]
ignore = [
    # pycodestyle
    'E741',
    # flake8-bugbear
    'B006',
    'B023',
    # flake8-bugbear opinionated (disabled by default in flake8)
    'B904',
    'B905',
    # pygrep-hooks
    "PGH003",
    # flake8-bandit
    "S101",  # assert used
    "S105",  # possible hardcoded password
    "S113",  # probable use of requests call without timeout
    "S324",  # probable use of insecure hash functions
    # flake8-simplify
    "SIM102", # nested 'if' statements
    "SIM103", # return condition directly
    "SIM105", # use contextlib.suppress
    "SIM108", # use ternary operator
    "SIM117", # use single 'with' statement
]
external = [  # Whitelist for RUF100 unkown code warnings
    "E704",
    "W291",
    "W293",
    "SIM110",
    "SIM113",
]
select = [
    "E",   # pycodestyle
    "F",   # Pyflakes
    "W",   # pycodestyle
    # plugins:
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "PGH", # pygrep-hooks
    "PLC", # pylint
    "PLE", # pylint
    "PLR", # pylint
    "PLW", # pylint
    "S",   # flake8-bandit
    "SIM", # flake8-simplify
    "T10", # flake8-debugger
    "UP",  # pyupgrade
    "RUF100",  # yesqa
]
```


</details>

---

_Comment by @AA-Turner on 2023-01-07 22:51_

> When you ran `ruff bug.py --diff `, what other error codes were enabled? (Since there's no `--select I`.)

`flake8`'s ``--isolated`` would be useful in general for bug reporting, as it means hidden config state can't affect the output of Ruff -- I reached for it the first few times I wanted to report a bug but it doesn't exist (yet!).

A

---

_Comment by @charliermarsh on 2023-01-07 22:53_

Would that force Ruff to use the default settings (+ whatever's passed in on the command-line)?

---

_Comment by @charliermarsh on 2023-01-07 22:53_

Ahh cool. Yes, seeing that now in Flake8. That would be straightforward to support.

---

_Comment by @charliermarsh on 2023-01-07 22:54_

Tracking here: https://github.com/charliermarsh/ruff/issues/1724.

---

_Comment by @charliermarsh on 2023-01-07 22:54_

With regards to the above: I'm just gonna chalk it up to Ruff fixing other errors (like the unused `pprint`) for now. If you see other confusing behavior, feel free to open another issue :)

---

_Comment by @AA-Turner on 2023-01-07 22:56_

Brilliant, thank you!

A

---
