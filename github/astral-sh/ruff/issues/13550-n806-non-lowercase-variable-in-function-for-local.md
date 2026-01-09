---
number: 13550
title: N806 (non-lowercase-variable-in-function) for local variable in function
type: issue
state: closed
author: opk12
labels:
  - question
assignees: []
created_at: 2024-09-29T18:16:27Z
updated_at: 2024-10-01T11:19:22Z
url: https://github.com/astral-sh/ruff/issues/13550
synced_at: 2026-01-07T13:12:15-06:00
---

# N806 (non-lowercase-variable-in-function) for local variable in function

---

_Issue opened by @opk12 on 2024-09-29 18:16_

mypy supports `Final` for local variables - it errors out, if `DIR` is assigned twice.

Constants should be uppercase, but ruff gives `N806` [non-lowercase-variable-in-function](https://docs.astral.sh/ruff/rules/non-lowercase-variable-in-function).

```
import pathlib
from typing import Final


def require_files() -> None:
    DIR: Final = pathlib.Path("/...")

    if not (DIR / "file_1").exists():
        print("Missing file 1")

    if not (DIR / "file_2").exists():
        print("Missing file 2")
```

```
$ ruff  check a.py  --no-cache  --select ALL  --preview --isolated
a.py:6:5: N806 Variable `DIR` in function should be lowercase
```

---

_Comment by @AlexWaygood on 2024-09-29 20:36_

Thanks for the issue! Hmm, `DIR` here isn't actually a constant, though, right? Since it's a local variable, you'll be creating a new `DIR` variable each time you call the function. I think you could make both Ruff and mypy happy here by moving it out of the function into the global scope (and your code would probably run slightly faster too!).

---

_Label `question` added by @AlexWaygood on 2024-09-29 20:36_

---

_Comment by @opk12 on 2024-10-01 09:31_

Sorry for being unclear above. The code passes mypy ([gist](https://mypy-play.net/?mypy=latest&python=3.12&gist=c0e2064b8f69e27af164db0e41864b79)). ruff is not aligned to the convention that mypy already enforces on both globals and locals. In fact, mypy errors out if a `Final` local is re-assigned ([gist](https://mypy-play.net/?mypy=latest&python=3.12&gist=2e6774b39fc9611d7e26df7667615eb9)), because it is certainly a bug.

In case of a "heavy" object's initialization, I can see how the [premature optimization](https://en.wikipedia.org/wiki/Program_optimization#When_to_optimize) of moving the implementation detail out of the function can help, at the expense of readability. But this is a general case, which I stumble in for simple types, and `Final` and the all-caps are intended to be sort of a "noqa".

---

_Comment by @AlexWaygood on 2024-10-01 09:48_

Sure. But if it's a local variable, then by definition it's not a constant, since you're creating it fresh every time the function is called. It's just a `Final` local variable -- so I think Ruff's complaint is correct here :-)

Only global constants that are created once should be uppercase, not local variables that are created every time the function is called. `Final` indicates that a variable is not mutated after being initialised -- this can be thought of as "necessary but not sufficient" for a variable to satisfy the definition of a global constant.

The naming convention here derives from [the relevant section in PEP 8](https://peps.python.org/pep-0008/#constants), which says:

> Constants are usually defined on a module level and written in all capital letters with underscores separating words. Examples include `MAX_OVERFLOW` and `TOTAL`.

In other words, I think either of these would satisfy both mypy and Ruff:

```py
import pathlib
from typing import Final


DIR: Final = pathlib.Path("/...")


def require_files() -> None:
    if not (DIR / "file_1").exists():
        print("Missing file 1")

    if not (DIR / "file_2").exists():
        print("Missing file 2")
```

or this:

```py
import pathlib
from typing import Final


def require_files() -> None:
	dir: Final = pathlib.Path("/...")

    if not (dir / "file_1").exists():
        print("Missing file 1")

    if not (dir / "file_2").exists():
        print("Missing file 2")
```

---

_Comment by @opk12 on 2024-10-01 11:19_

Yes, on a second thought, if distinguishing vars from constants is necessary for readability, it likely means the function is doing too many things. Thank you very much for your answers.

---

_Closed by @opk12 on 2024-10-01 11:19_

---

_Referenced in [astral-sh/ruff#15721](../../astral-sh/ruff/issues/15721.md) on 2025-01-24 18:43_

---
