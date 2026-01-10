```yaml
number: 14127
title: " logging.config not being detected by F401"
type: issue
state: closed
author: michelkluger
labels: []
assignees: []
created_at: 2024-11-06T10:04:04Z
updated_at: 2024-11-06T11:39:19Z
url: https://github.com/astral-sh/ruff/issues/14127
synced_at: 2026-01-10T11:09:55Z
```

#  logging.config not being detected by F401

---

_Issue opened by @michelkluger on 2024-11-06 10:04_

is this the expected behaviour? if yes, why?

```
❯ ruff check --select F example.py
example.py:3:18: F401 [*] `pandas` imported but unused
  |
1 | import logging
2 | import logging.config
3 | import pandas as pd
  |                  ^^ F401
4 |
5 | logger = logging.getLogger(__name__)
  |
  = help: Remove unused import: `pandas`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

ruff==0.7.2

---

_Comment by @MichaReiser on 2024-11-06 11:39_

This is a bug, see #60 and https://github.com/astral-sh/ruff/pull/13965

---

_Closed by @MichaReiser on 2024-11-06 11:39_

---
