```yaml
number: 18606
title: "[Infinite loop]"
type: issue
state: closed
author: marcelatpwc
labels:
  - needs-mre
assignees: []
created_at: 2025-06-10T08:12:38Z
updated_at: 2025-06-23T07:27:21Z
url: https://github.com/astral-sh/ruff/issues/18606
synced_at: 2026-01-10T11:09:58Z
```

# [Infinite loop]

---

_Issue opened by @marcelatpwc on 2025-06-10 08:12_

ruff 0.11.13, rule code F401

```py
from . import (
    # relative imports
)
```

```pyproject.toml
[tool.ruff]
unsafe-fixes = true

[tool.ruff.lint]
preview = true
select = ["ALL", "I001", "I002"]
fixable = ["ALL", "I001", "I002"]
ignore = [
  "PD901",
  "D104",
  "D107",
  "D102",
  "FA100",
  "D205",
  "D203",
  "D212",
  "COM812",
  "ANN003",
  "E501",
  "CPY001",
  "UP007",
  "FA102",
  "D100",
  "TD003",
  "D103",
  "E712",
]
```



---

_Comment by @MichaReiser on 2025-06-10 09:09_

I tried to reproduce this but there are no suggested fixes for the given example. Could you provide a more complete code exmaple?

What I tried:

```bash
 uvx ruff check create.py --select ALL --fix --fixable=ALL --preview
warning: `incorrect-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `incorrect-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
create.py:1:1: CPY001 Missing copyright notice at top of file
create.py:3:1: SyntaxError: Expected one or more symbol names after import
  |
1 | from . import (
2 |     # relative imports
3 | )
  | ^
  |

create.py:3:2: W292 No newline at end of file
  |
1 | from . import (
2 |     # relative imports
3 | )
  |  ^ W292
  |
  = help: Add trailing newline

Found 3 errors.
```

---

_Label `needs-mre` added by @MichaReiser on 2025-06-10 09:09_

---

_Closed by @MichaReiser on 2025-06-23 07:27_

---
