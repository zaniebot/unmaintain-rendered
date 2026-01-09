---
number: 8790
title: Add rule to suggest f-string over string concatenation
type: issue
state: closed
author: ThiefMaster
labels: []
assignees: []
created_at: 2023-11-20T16:26:34Z
updated_at: 2023-11-20T16:32:26Z
url: https://github.com/astral-sh/ruff/issues/8790
synced_at: 2026-01-07T13:12:15-06:00
---

# Add rule to suggest f-string over string concatenation

---

_Issue opened by @ThiefMaster on 2023-11-20 16:26_

```py
prefix = 'https://example.com'
print(prefix + '/test')
```

even with `--select ALL --preview` i only get unrelated results:

```py
$ ruff --isolated --select ALL --preview --no-cache rufftest/ruff_sample.py
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
rufftest/ruff_sample.py:1:1: INP001 File `rufftest/ruff_sample.py` is part of an implicit namespace package. Add an `__init__.py`.
rufftest/ruff_sample.py:1:1: D100 Missing docstring in public module
rufftest/ruff_sample.py:1:1: CPY001 Missing copyright notice at top of file
rufftest/ruff_sample.py:1:10: Q000 [*] Single quotes found but double quotes preferred
rufftest/ruff_sample.py:2:1: T201 `print` found
rufftest/ruff_sample.py:2:16: Q000 [*] Single quotes found but double quotes preferred
Found 6 errors.
[*] 2 fixable with the `--fix` option.

```

---

_Comment by @charliermarsh on 2023-11-20 16:32_

I believe this is the same as `FLY001` proposed in https://github.com/astral-sh/ruff/issues/2102.

---

_Closed by @charliermarsh on 2023-11-20 16:32_

---
