```yaml
number: 19159
title: "`match_docstring_end` matches too much, making I002 introduce a syntax error"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-06T13:46:03Z
updated_at: 2025-07-11T16:35:43Z
url: https://github.com/astral-sh/ruff/issues/19159
synced_at: 2026-01-10T11:09:59Z
```

# `match_docstring_end` matches too much, making I002 introduce a syntax error

---

_Issue opened by @dscorbett on 2025-07-06 13:46_

### Summary

When multiple string-only statements appear at the top of a module, class, or function definition, only the first is the docstring, but [`match_docstring_end`](https://github.com/astral-sh/ruff/blob/0.12.2/crates/ruff_linter/src/importer/insertion.rs#L278) matches them all. That makes the fix for [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) insert required imports after all the string-only statements, not immediately after the docstring. That is usually harmless but it introduces a syntax error when the required import is a future import. [Example](https://play.ruff.rs/edc189cf-38b9-4423-b2b5-f56d458b53f7):

```console
$ cat >i002.py <<'# EOF'
"docstring"
"not a docstring"
print(f"{__doc__=}")
# EOF

$ python i002.py
__doc__='docstring'

$ ruff --isolated check --config 'lint.isort.required-imports=["from __future__ import annotations"]' i002.py --select I002 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat i002.py
"docstring"
"not a docstring"
from __future__ import annotations
print(f"{__doc__=}")

$ python i002.py 2>&1 | tail -n 1
SyntaxError: from __future__ imports must occur at the beginning of the file
```

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)

---

_Label `bug` added by @ntBre on 2025-07-07 20:59_

---

_Label `fixes` added by @ntBre on 2025-07-07 20:59_

---

_Closed by @MichaReiser on 2025-07-11 16:35_

---
