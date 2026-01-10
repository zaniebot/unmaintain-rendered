---
number: 19498
title: "`start_of_file` ignores a backslash, making I002 introduce a syntax error"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-07-22T20:35:32Z
updated_at: 2025-07-30T16:12:47Z
url: https://github.com/astral-sh/ruff/issues/19498
synced_at: 2026-01-10T01:23:00Z
---

# `start_of_file` ignores a backslash, making I002 introduce a syntax error

---

_Issue opened by @dscorbett on 2025-07-22 20:35_

### Summary

[`start_of_file`](https://github.com/astral-sh/ruff/blob/0.12.4/crates/ruff_linter/src/importer/insertion.rs#L53) ignores a backslash after a docstring, making the fix for [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) insert the required import immediately after the docstring, on a new physical line but the same logical line, which is a syntax error. [Example](https://play.ruff.rs/06e177f2-76f7-4712-82ae-549bcf3973f0):
```console
$ cat >i002.py <<'# EOF'
"docstring"\

print(f"{__doc__=}")
# EOF

$ ruff --isolated check i002.py --select I002 --config 'lint.isort.required-imports=["import sys"]' --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

The erroring output in that example is:
```python
"docstring"\
import sys

print(f"{__doc__=}")
```

The output should be:
```python
"docstring"\

import sys
print(f"{__doc__=}")
```

### Version

ruff 0.12.4 (ee2759b36 2025-07-17)

---

_Label `bug` added by @ntBre on 2025-07-22 21:32_

---

_Label `fixes` added by @ntBre on 2025-07-22 21:32_

---

_Label `help wanted` added by @ntBre on 2025-07-22 21:32_

---

_Referenced in [astral-sh/ruff#19505](../../astral-sh/ruff/pulls/19505.md) on 2025-07-23 10:32_

---

_Comment by @jimhoekstra on 2025-07-23 11:39_

Hi! I submitted a PR for this issue. This is my first time contributing to an open source project, let me know what you think :)
PR: https://github.com/astral-sh/ruff/pull/19505

---

_Closed by @ntBre on 2025-07-30 16:12_

---
