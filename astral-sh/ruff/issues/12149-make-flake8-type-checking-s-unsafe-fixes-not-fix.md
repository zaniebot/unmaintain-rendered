```yaml
number: 12149
title: "Make flake8-type-checking's unsafe fixes not fix if a library is used in a doctest"
type: issue
state: open
author: mikeweltevrede
labels:
  - docstring
assignees: []
created_at: 2024-07-02T12:09:10Z
updated_at: 2024-07-09T06:57:11Z
url: https://github.com/astral-sh/ruff/issues/12149
synced_at: 2026-01-12T15:54:51Z
```

# Make flake8-type-checking's unsafe fixes not fix if a library is used in a doctest

---

_@mikeweltevrede_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hi all, curious to hear your opinion on this :) Please let me know if this needs to be filed on flake8-type-checking instead!

# Ruleset
flake8-type-checking (TCH) with unsafe fixes on.

# Ruff call and settings
## Ruff version
0.4.8

## Command invoked
pre-commit hooks:
```yaml
repos:
  - repo: local
    hooks:
      - id:       ruff
        name:     Run ruff linter
        entry:    ruff
        args:     [ "--no-cache" ]
        language: python
        types:    [ file, python ]
        files:    .*\.py$
```

Relevant settings in `pyproject.toml`:
```toml
[tool.ruff]
line-length = 120
target-version = "py39"
fix = true  # Allow for ruff linting to fix some issues. Note that we control this behaviour in [tool.ruff.lint].

[tool.ruff.lint]
extend-select = [
    "TCH",  # flake8-type-checking | Allow for imports only used for type hinting | https://docs.astral.sh/ruff/rules/#flake8-type-checking-tch
]
fixable = [
    "TCH001", "TCH002", "TCH003", "TCH004", "TCH005",   # flake8-type-checking
]
extend-safe-fixes = [
    "TCH001", "TCH002", "TCH003", "TCH004", "TCH005",   # flake8-type-checking
]

[tool.ruff.lint.flake8-type-checking]
quote-annotations = true
strict = true
exempt-modules = ["typing", "typing_extensions", "types"]
```

# Explanation and Example
When allowing TCH to auto-fix, it will transform this...
```python
from datetime import datetime

def my_func(dt: datetime):
    """Example function

    >>> my_func(dt=datetime(year=2024, month=7, day=2))
    'hello'
    """
    return "hello"


if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

into ...
```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from datetime import datetime

def my_func(dt: "datetime"):
    """Example function

    >>> my_func(dt=datetime(year=2024, month=7, day=2))
    'hello'
    """
    return "hello"


if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

While this seems correct at first glance (no SyntaxError), notice that `datetime` is used in the doctest. It would be great if the ruff fixer can scan doctests as well for usage.

---

_Label `docstring` added by @MichaReiser on 2024-07-03 06:34_

---

_Comment by @MichaReiser on 2024-07-03 06:37_

It makes sense to me that Ruff shouldn't provide an autofix in this case because it breaks user code. Doing this would require that Ruff understands doctests and takes them into account when running the analysis. How this would work is an interesting question. 

---

_Comment by @mikeweltevrede on 2024-07-09 06:57_

@MichaReiser Indeed, it is relatively strange behaviour and therefore it makes sense that it is an unsafe fix.

Doctests are well-defined in Python, with `>>>` being included at the start of a new line in the docstring. The doctest library (or pytest with the flag `--doctest-modules`) will then recognize these and run them as tests.

It is interesting to think about whether there are use cases where people might use `>>>` at the start of a new line where it is not intended to be a doctest. Does this happen? Are these valid use cases? Is this just ignorance (I learned about doctests like this a while back but before this thought they were just examples, not meant to be runnable code per se)? Should Ruff provide an opinion here? Either way, this should remain an unsafe fix, keeping these points in mind.

I do not know how to program in Rust at the moment but if this would be implemented, I am happy to assist the person working on this and think along!

---
