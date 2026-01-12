```yaml
number: 10202
title: "`# flake8: noqa` top-of-the-file comment doesn't respect `external` config"
type: issue
state: closed
author: Avasam
labels:
  - bug
assignees: []
created_at: 2024-03-02T18:13:47Z
updated_at: 2024-03-03T00:20:37Z
url: https://github.com/astral-sh/ruff/issues/10202
synced_at: 2026-01-12T15:54:50Z
```

# `# flake8: noqa` top-of-the-file comment doesn't respect `external` config

---

_@Avasam_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The warning:
`warning: Invalid rule code provided to `# ruff: noqa` at stubs\six\six\moves\builtins.pyi:1: NQA102`

The Python file:
```py
# flake8: noqa: NQA102 # https://github.com/plinss/flake8-noqa/issues/22
# six explicitly re-exports builtins. Normally this is something we'd want to avoid.
# But this is specifically a compatibility package.
from builtins import *  # noqa: UP029
```

The Ruff lint configs (I stripped out format configs):
```toml

[tool.ruff.lint]
# We still use flake8-pyi and flake8-noqa to check these (see .flake8 config file)
external = ["F821", "NQA", "Y"]
extend-select = [
    "B", # flake8-bugbear
    "FA", # flake8-future-annotations
    "I", # isort
    "RUF", # Ruff-specific and unused-noqa
    "UP", # pyupgrade
    # PYI: only enable rules that always autofix, avoids duplicate # noqa with flake8-pyi
    # See https://github.com/plinss/flake8-noqa/issues/22
    "PYI009", # use `...`, not `pass`, in empty class bodies
    "PYI010", # function bodies must be empty
    "PYI012", # class bodies must not contain `pass`
    "PYI013", # non-empty class bodies must not contain `...`
    "PYI016", # duplicate union member
    "PYI020", # quoted annotations are always unnecessary in stubs
    "PYI025", # always alias `collections.abc.Set` as `AbstractSet` when importing it
    "PYI032", # use `object`, not `Any`, as the second parameter to `__eq__`
    "PYI055", # multiple `type[T]` usages in a union
    "PYI058", # use `Iterator` as the return type for `__iter__` methods
]
ignore = [
    ###
    # Rules we don't want or don't agree with
    ###
    # Slower and more verbose https://github.com/astral-sh/ruff/issues/7871
    "UP038", # Use `X | Y` in `isinstance` call instead of `(X, Y)`
    # Used for direct, non-subclass type comparison, for example: `type(val) is str`
    # see https://github.com/astral-sh/ruff/issues/6465
    "E721", # Do not compare types, use `isinstance()`
    ###
    # False-positives, but already checked by type-checkers
    ###
    # Ruff doesn't support multi-file analysis yet: https://github.com/astral-sh/ruff/issues/5295
    "RUF013", # PEP 484 prohibits implicit `Optional`
]
```


---

_Label `bug` added by @charliermarsh on 2024-03-02 22:52_

---

_Comment by @charliermarsh on 2024-03-02 22:52_

Good call!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-02 23:57_

---

_Closed by @charliermarsh on 2024-03-03 00:20_

---
