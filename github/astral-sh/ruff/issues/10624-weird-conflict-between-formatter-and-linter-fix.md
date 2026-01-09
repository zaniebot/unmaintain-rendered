---
number: 10624
title: Weird conflict between formatter and linter fix all
type: issue
state: open
author: mgzenitech
labels:
  - isort
  - formatter
  - incompatibility
assignees: []
created_at: 2024-03-27T08:59:03Z
updated_at: 2025-02-03T14:29:25Z
url: https://github.com/astral-sh/ruff/issues/10624
synced_at: 2026-01-07T13:12:15-06:00
---

# Weird conflict between formatter and linter fix all

---

_Issue opened by @mgzenitech on 2024-03-27 08:59_

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

Here are two files:
```
#!/usr/bin/env -S poetry run python

from typing import Any
from toml import load

# @BUG: https://github.com/darrenburns/ward/issues/378
from ward import Scope, fixture  # pyright: ignore [reportUnknownVariableType]
```

and

```
#!/usr/bin/env -S poetry run python

from collections.abc import Callable
from typing import Any
from unittest.mock import MagicMock

# @BUG: https://github.com/darrenburns/ward/issues/378
from ward import Scope, fixture  # pyright: ignore [reportUnknownVariableType]
```

First one doesn't have any issues yet the second one is triggering `I001`,

.ruff.toml:
```
builtins = [
  "_"
]
cache-dir = "tmp/ruff"
preview = true
target-version = "py312"
unsafe-fixes = true

[format]
docstring-code-format = true
docstring-code-line-length = 120
line-ending = "lf"

[lint]
dummy-variable-rgx = "^_$"
extend-select = [
  "D213"
]
ignore = [
  "CPY001",
  "D212"
]
select = [
  "ALL"
]
task-tags = [
  "@BUG",
  "@HACK",
  "@TODO"
]

[lint.isort]
combine-as-imports = true
force-sort-within-sections = true
lines-after-imports = 2
no-lines-before = [
  "first-party",
  "future",
  "local-folder",
  "standard-library",
  "third-party"
]

[lint.pycodestyle]
max-doc-length = 120
max-line-length = 120

[lint.pydocstyle]
convention = "google"

[lint.pylint]
allow-magic-value-types = [
]

[lint.flake8-annotations]
allow-star-arg-any = true
```

---

_Comment by @MichaReiser on 2024-03-27 09:28_

Thanks for reporting this bug. 

The issue here is that the formatter enforces a blank line between two imports if there's an own-line comment between them (similar to black).

```py
from unittest.mock import MagicMock
# @BUG: https://github.com/darrenburns/ward/issues/378
from ward import Scope, fixture  # pyright: ignore [reportUnknownVariableType]
```

Gets reformatted to 

```py
from unittest.mock import MagicMock

# @BUG: https://github.com/darrenburns/ward/issues/378
from ward import Scope, fixture  # pyright: ignore [reportUnknownVariableType]
```

Now, `isort` doesn't agree with the blank line because of the `no-lines-before`  configuration. 

---

_Label `isort` added by @MichaReiser on 2024-03-27 09:28_

---

_Label `formatter` added by @MichaReiser on 2024-03-27 09:28_

---

_Label `incompatibility` added by @MichaReiser on 2024-03-27 09:28_

---

_Comment by @mgzenitech on 2024-03-27 09:34_

Will formatter be updated to take into account .ruff.toml config of isort?

---

_Comment by @MichaReiser on 2024-03-27 09:51_

I need to think in more detail about what the best solution here is. We want to avoid that the `formatter` needs to read the `isort` configuration and reimplement the same logic (the formatter doesn't know about first-party-imports etc). 

---

_Comment by @mgzenitech on 2024-03-27 14:59_

I think I've found a bunch of other issues too with formatter conflicting lint fix. Long term formatter should be compatible with linter config file...

---

_Comment by @charliermarsh on 2024-03-27 15:01_

For what it's worth, by far the best way to ensure compatibility is to avoid highly customized formatting rules or configuration in the linter (e.g., modifying the `isort` settings). It's possible that we don't end up "fixing" those and instead just mark them as incompatible with user-facing warnings, as with other formatter-conflicting lint rules and settings (see: https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules).

---

_Comment by @ERW105 on 2025-02-02 10:30_

> I need to think in more detail about what the best solution here is. We want to avoid that the `formatter` needs to read the `isort` configuration and reimplement the same logic (the formatter doesn't know about first-party-imports etc).

In the original isort, it appears that `ensure_newline_before_comments` takes priority over `no_lines_before`, such that there is always a newline before comments and the output is black-compatible. Including when a comment is before the first import of a `no_lines_before` section.

`ensure_newline_before_comments` is part of isort's black profile and I assume the behavior ruff implements implicitly. Perhaps this divergence is unintended? I don't think we need the formatter to be involved here.

Super impressed with ruff and would love to keep using `no-lines-before`!


---

_Comment by @MichaReiser on 2025-02-03 14:29_

Nice find @ERW105 Yes, implementing the behavior of `ensure_newline_before_comments` -- at least if `no_lines_before = 0` (without adding an option) is probably the solution here, although it is a breaking change.

---
