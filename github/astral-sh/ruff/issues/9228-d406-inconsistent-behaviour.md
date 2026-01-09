---
number: 9228
title: D406 inconsistent behaviour
type: issue
state: closed
author: chaitanya2692
labels:
  - question
  - docstring
assignees: []
created_at: 2023-12-21T10:10:30Z
updated_at: 2024-01-09T03:13:38Z
url: https://github.com/astral-sh/ruff/issues/9228
synced_at: 2026-01-07T13:12:15-06:00
---

# D406 inconsistent behaviour

---

_Issue opened by @chaitanya2692 on 2023-12-21 10:10_

I am running ruff on the following example code 

```
"""Toy Example."""
import json
from pathlib import Path


class MyClass:
    """Toy Class."""

    def __init__(self):
        """Initialize an instance of the class."""

    def get_values(self):
        """Get info on person.

        Returns:
        -------
            Dictionary of results
        """
        return {
            "name": "John Doe",
            "age": 30,
        }


def json_output(my_file: str):
    """Read target json file.

    Args:
    ----
        my_file: path to target json file

    Raises:
    ------
        Exception: if json file could not be found.

    Returns:
    -------
        The content of json.
    """
    json_file = "my_file.json"

    if not Path(json_file).is_file():
        raise Exception(f"Could not find json file: {json_file}")

    print("Json file found:", json_file)
    with open(json_file, "r") as file:
        return json.load(file)

```

The output of the linter is as below
`test.py:13:9: D406 [*] Section name should end with a newline ("Returns")`

If there's a fault, why is only one of the return statements flagged here?

The command I use is `ruff check --preview --config ci/dockerfiles/linting/ruff.toml test.py`

Here's my current ruff.toml:

```
line-length = 88

[lint]
preview = true
ignore = ["E203","E231"]
select = [  # https://docs.astral.sh/ruff/rules
    "B",   # flake8-bugbear
    "D",   # pydocstyle
    "ERA", # eradicate
    "PT",  # flake8-pytest-style
    "RET", # flake8-return
    "A",   # flake8_builtins
    "I",   # isort
    "C90", # mccabe
    "N",   # pep8-naming
    "E",   # pycodestyle
    "W",   # pycodestyle
    "F",   # pyflakes
    # No replacement for flake8-darglint yet
]

[mccabe]
max-complexity = 10
```

My current ruff version is `0.1.7`. `0.1.8` seems to have the same error.

---

_Comment by @charliermarsh on 2023-12-21 20:01_

This is just a confusing thing that comes from not setting a `pydocstyle.convention` (https://docs.astral.sh/ruff/settings/#pydocstyle-convention)... Basically, we don't respect arbitrary section headers -- they need to match either the list of valid Google or NumPy section headers. And if you don't set a convention, then we try to infer from the existing headers. In the first docstring, we infer NumPy, because we can't differentiate it -- and that leads us to look at `Returns:`, and flag it. (You can fix this error by removing the colon, by the way.)

In the second docstring, we see `Args`, which is _not_ a valid NumPy header. It only exists in the Google convention. (In NumPy, it's `Parameters`.) So we treat that docstring as a Google docstring, in which case, you're allowed to end the line with a colon.

I'd recommend setting a convention for more consistent behavior.

---

_Label `question` added by @charliermarsh on 2023-12-21 20:01_

---

_Label `docstring` added by @charliermarsh on 2023-12-21 20:01_

---

_Closed by @charliermarsh on 2024-01-09 03:13_

---
