---
number: 13524
title: PLR1716 fix introduces a syntax error with unbalanced parentheses
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-09-26T13:35:32Z
updated_at: 2024-12-09T04:58:46Z
url: https://github.com/astral-sh/ruff/issues/13524
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR1716 fix introduces a syntax error with unbalanced parentheses

---

_Issue opened by @dscorbett on 2024-09-26 13:35_

When the two expressions to be collapsed by [`boolean-chained-comparison` (PLR1716)](https://docs.astral.sh/ruff/rules/boolean-chained-comparison/) are parenthesized with different numbers of parentheses, the fix makes Ruff fail.

```console
$ ruff --version
ruff 0.6.8
$ echo '(a < b) and b < c' | ruff check --isolated --preview --select PLR1716 - --fix

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `-`, the rule codes PLR1716, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

(a < b) and b < c
-:1:2: PLR1716 Contains chained boolean comparison that can be simplified
  |
1 | (a < b) and b < c
  |  ^^^^^^^^^^^^^^^^ PLR1716
  |
  = help: Use a single compare expression

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Label `bug` added by @AlexWaygood on 2024-09-26 14:07_

---

_Label `fixes` added by @AlexWaygood on 2024-09-26 14:07_

---

_Assigned to @zanieb by @zanieb on 2024-09-26 14:32_

---

_Comment by @zanieb on 2024-09-26 15:01_

Ah this looks like.. hard to fix. We can at least not offer an invalid fix here.

---

_Referenced in [astral-sh/ruff#13527](../../astral-sh/ruff/pulls/13527.md) on 2024-09-26 15:02_

---

_Comment by @dscorbett on 2024-10-04 14:33_

This was addressed in version 0.6.9. Is there anything left to do for this issue?

---

_Comment by @zanieb on 2024-10-04 14:35_

We don't offer the fix, but we can do better and offer the fix with proper handling.

---

_Assigned to @dylwil3 by @dylwil3 on 2024-12-04 18:14_

---

_Referenced in [astral-sh/ruff#14781](../../astral-sh/ruff/pulls/14781.md) on 2024-12-05 04:11_

---

_Closed by @dylwil3 on 2024-12-09 04:58_

---

_Closed by @dylwil3 on 2024-12-09 04:58_

---
