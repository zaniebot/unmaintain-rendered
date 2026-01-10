```yaml
number: 10547
title: Mark PYI025 fix as safe in more cases for stub files
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
assignees: []
merged: true
base: main
head: safe-AbstractSet-rename
created_at: 2024-03-24T14:53:41Z
updated_at: 2024-03-24T16:11:49Z
url: https://github.com/astral-sh/ruff/pull/10547
synced_at: 2026-01-10T22:47:02Z
```

# Mark PYI025 fix as safe in more cases for stub files

---

_Pull request opened by @AlexWaygood on 2024-03-24 14:53_

## Summary

The fix for PYI025 is currently marked as unsafe in non-global scopes for both `.py` and `.pyi` files, on the grounds that all global-scope symbols in Python are implicitly exported from the module, so changing the name of something in the global scope could break other modules that import the module we're fixing. Unlike in `.py` files, however, imported symbols are never implicitly re-exported from stub files. Symbols are only understood by static analysis tools as being re-exported from stubs if they are marked as explicit re-exports, which take three forms:

```py
from foo import *  # all symbols from foo are re-exported from the stub

# the "redundant" alias marks it as an explicit re-export
# (note that the alias needs to be identical to the symbol's "actual" name
# in order for it to be a re-export)
from bar import barrr as barrr

# inclusion in __all__ also marks it as an explicit re-export,
# just like in `.py` files
from baz import bazzz
__all__ = ["bazzz"]
```

This is [specc'd in PEP 484](https://peps.python.org/pep-0484/#stub-files), and means that we can mark the fix for PYI025 as safe in more cases for `.pyi` files.

## Test Plan

`cargo test`. An existing test case goes from being an unsafe fix to a safe fix in a `.pyi` fixture. I also added a new fixture so we have coverage of global-scope imports that are marked as re-exports using "redundant" `from collections.abc import Set as Set` aliases.


---

_Comment by @AlexWaygood on 2024-03-24 14:55_

Fun fact: I wrote a version of this PR on Thursday, but bumped into #10508 while writing tests for it. That then meant I had to do https://github.com/astral-sh/ruff/pull/10525 and https://github.com/astral-sh/ruff/pull/10527 first, before I could file this 😄

---

_Comment by @github-actions[bot] on 2024-03-24 15:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @AlexWaygood on 2024-03-24 15:41_

---

_@charliermarsh approved on 2024-03-24 16:08_

---

_Merged by @AlexWaygood on 2024-03-24 16:11_

---

_Closed by @AlexWaygood on 2024-03-24 16:11_

---

_Branch deleted on 2024-03-24 16:11_

---
