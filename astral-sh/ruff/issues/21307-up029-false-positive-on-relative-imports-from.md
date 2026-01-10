```yaml
number: 21307
title: "`UP029` false positive on relative imports from local `.builtins` module"
type: issue
state: closed
author: amyreese
labels: []
assignees: []
created_at: 2025-11-06T21:43:25Z
updated_at: 2025-11-07T16:01:53Z
url: https://github.com/astral-sh/ruff/issues/21307
synced_at: 2026-01-10T11:10:00Z
```

# `UP029` false positive on relative imports from local `.builtins` module

---

_Issue opened by @amyreese on 2025-11-06 21:43_

### Summary

Discovered when running ruff on [aioitertools](https://aioitertools.omnilib.dev/en/stable/index.html), which reimplements several builtins as async-compatible variants, where `UP029` incorrectly flags imports from the local `.builtins` module as unnecessary imports:

```
amethyst@espeon ~/workspace/aioitertools ruff+ » cat foo.py
from .builtins import next

amethyst@espeon ~/workspace/aioitertools ruff+ » uv run ruff check --isolated --select UP029 foo.py
UP029 Unnecessary builtin import: `next`
 --> foo.py:1:1
  |
1 | from .builtins import next
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Remove unnecessary builtin import

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
> [1]
```

### Version

ruff 0.14.3

---

_Closed by @MichaReiser on 2025-11-07 16:01_

---
