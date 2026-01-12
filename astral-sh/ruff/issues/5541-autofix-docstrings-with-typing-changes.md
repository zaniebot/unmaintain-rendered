```yaml
number: 5541
title: Autofix docstrings with typing changes?
type: issue
state: open
author: petermattia
labels:
  - docstring
  - wish
assignees: []
created_at: 2023-07-05T20:02:08Z
updated_at: 2023-12-21T21:59:27Z
url: https://github.com/astral-sh/ruff/issues/5541
synced_at: 2026-01-12T15:54:45Z
```

# Autofix docstrings with typing changes?

---

_@petermattia_

First off, thanks for `ruff`, really an amazing utility for the python ecosystem!

We're slowly enabling more rules with `ruff`. We just enabled the `UP` rule set, which mostly caught outdated typing conventions. Wondering if it would be possible to (ideally) auto-fix the docstrings or perhaps somehow warn the user that the docstrings may require updating as well?

Example: 
![image](https://github.com/astral-sh/ruff/assets/29551858/e47099bd-8d3f-452e-8328-7d48034206b8)

* A minimal code snippet that reproduces the bug.

`ruff .`

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

`ruff .`

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

We're using `numpy`-style docstrings. Also, the `UP` set of rules is added.
````
[tool.ruff]
select = ["UP", ...]

[tool.ruff.pydocstyle]
convention = "numpy"
````

* The current Ruff version (`ruff --version`).

`0.0.275`


---

_Comment by @charliermarsh on 2023-07-05 20:18_

This is a really nice idea... It's probably a ways off, I think we need a proper docstring parser to support it (rather than the pydocstyle-inspired regular expressions we use now), but it would be really cool.

---

_Label `docstring` added by @charliermarsh on 2023-07-05 20:19_

---

_Comment by @petermattia on 2023-07-05 22:11_

No worries, thank you!

---

_Comment by @smackesey on 2023-07-06 11:27_

I would love this feature-- huge value-add in terms of ensuring our API docs are accurate.

FWIW robust docstring parsing/formatting/linting would be a huge feather in ruff's cap, the existing tooling ecosystem for this is terrible given what a universal problem it is.

---

_Label `wish` added by @charliermarsh on 2023-07-10 01:22_

---

_Comment by @jorgegomezcq on 2023-07-28 16:24_

> FWIW robust docstring parsing/formatting/linting would be a huge feather in ruff's cap, the existing tooling ecosystem for this is terrible given what a universal problem it is.

darglint is a related tool but it looks like it was deprecated last December:

https://github.com/terrencepreilly/darglint

There's also pyment but it has its rough edges that I've run into:

https://github.com/dadadel/pyment

These might be good projects to consult when implementing #5541

---

_Comment by @T-256 on 2023-10-13 02:02_

FWIW(?) I interested in [PEP 727](https://peps.python.org/pep-0727/) that may deprecate type definitions in docstrings in future.
```py
from typing import Annotated, Doc

def create_user(
    name: Annotated[str, Doc("The user's name")],
    age: Annotated[int, Doc("The user's age")],
    cursor: DatabaseConnection | None = None,
) -> Annotated[User, Doc("The created user after saving in the database")]:
    """Create a new user in the system.

    It needs the database connection to be already initialized.
    """
```

---

_Comment by @ShivMunagala on 2023-12-20 00:28_

pydoclint (https://github.com/jsh9/pydoclint) partly does this, it doesn't autofix afaik but it can warn a user if there is a mismatch between docstring and typehints.

---

_Comment by @petermattia on 2023-12-21 21:59_

Yea, we currently use pydoclint as a precommit hook. Generally good enough for our purposes, but would be nice to drop it and consolidate with ruff.

---
