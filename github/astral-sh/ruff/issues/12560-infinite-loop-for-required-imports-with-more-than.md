---
number: 12560
title: "[Infinite loop] for `required-imports` with more than one argument"
type: issue
state: closed
author: Anselmoo
labels:
  - needs-info
assignees: []
created_at: 2024-07-29T04:24:27Z
updated_at: 2024-07-29T19:42:34Z
url: https://github.com/astral-sh/ruff/issues/12560
synced_at: 2026-01-07T13:12:15-06:00
---

# [Infinite loop] for `required-imports` with more than one argument

---

_Issue opened by @Anselmoo on 2024-07-29 04:24_

It looks like a `required-import` fails for more than one import. Would this in general possible to import more than _required_ import?

```toml
required-imports = [
    "from __future__ import annotations",
    "import numpy as np",
]
```

and get:

> the contents of `mypackage/data_model.py`, the rule codes I002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!



<details><summary>Complete setup</summary>
<p>

```toml
[tool.ruff]
line-length = 88
indent-width = 4
target-version = "py39"
lint.select = [
    "ALL",
]
fix = true
lint.fixable = [
    "ALL",
]
lint.ignore = [
    "ANN101",  # annotations for self
    "ARG002",  # unused argument
    "PLR0913", # too many arguments
    "PD008",   # missing pandas
    "ISC001",  # single-line-implicit-string-concatenation 
    "COM812",  # missing trailing-comma"
]
src = [
    "mypackage",
]
exclude = [
    "mypackage/test",
    "mypackage/core",
]

[tool.ruff.format]
docstring-code-format = true
docstring-code-line-length = 60
indent-style = "space"
preview = true
quote-style = "double"
# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

[tool.ruff.lint.per-file-ignores]
"mypackage/test/test_*.py" = [
    "PLR2004", # magic value comparison
    "S101",    # use of assert detected
    "TCH002",  # third party import (for pytest)
]

[tool.ruff.lint.pydocstyle]
convention = "google"

[tool.ruff.lint.isort]
known-first-party = [
    "mypackage",
]
force-single-line = true
lines-between-types = 1
lines-after-imports = 2
known-third-party = [
    "poetry.core",
]
required-imports = [
    "from __future__ import annotations",
    "import numpy as np",
]
```

</p>
</details> 


---

_Comment by @MichaReiser on 2024-07-29 06:21_

Could you tell me more about what you mean by " fails for more than one import."? 

I can add more than one required import and ruff provides auto fixes to add them. 

https://play.ruff.rs/9eb55b5a-2879-4d03-9923-4687c8cd3814



---

_Label `needs-info` added by @MichaReiser on 2024-07-29 06:21_

---

_Comment by @Anselmoo on 2024-07-29 19:42_

@MichaReiser sorry for not being precise.

I have using a `pyproject.toml` together with poetry to setup it up. 

I have also cross-check with a different repo https://github.com/Anselmoo/useful-math-functions, so unfortunately my test repo is still private. I am fine to close the issue and will may later open it ...

---

_Closed by @Anselmoo on 2024-07-29 19:42_

---
