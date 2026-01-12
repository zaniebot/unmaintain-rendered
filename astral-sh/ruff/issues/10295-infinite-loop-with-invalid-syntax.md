```yaml
number: 10295
title: Infinite loop with invalid syntax
type: issue
state: closed
author: jc-louis
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-03-08T10:46:09Z
updated_at: 2024-03-11T21:23:19Z
url: https://github.com/astral-sh/ruff/issues/10295
synced_at: 2026-01-12T15:54:50Z
```

# Infinite loop with invalid syntax

---

_@jc-louis_

With this invalid syntax
```py
a = (1 or)
```

I get with ruff v0.3.1

```
error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `test.py`, the rule codes E225, E275, E999, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

error: Failed to parse test.py:1:10: Unexpected token ')'
test.py:1:8: E225 Missing whitespace around operator
  |
1 | a = (1 or)
  |        ^^ E225
  |
  = help: Add missing whitespace

test.py:1:8: E275 Missing whitespace after keyword
  |
1 | a = (1 or)
  |        ^^ E275
  |
  = help: Added missing whitespace after keyword

test.py:1:10: E999 SyntaxError: Unexpected token ')'
  |
1 | a = (1 or)
  |          ^ E999
  |

Found 153 errors (150 fixed, 3 remaining).
[*] 2 fixable with the `--fix` option.
```

Not sure if it is really a bug given the invalid syntax, but the output could be nicer.

---

_Comment by @dhruvmanila on 2024-03-08 11:18_

I think it's because the auto-fix for `E225` tries to add a whitespace after `or` making it `a = (1 or )` but then there's `E202` which tries to remove the whitespace before `)`. This makes it an infinite loop.

---

_Label `bug` added by @dhruvmanila on 2024-03-08 11:18_

---

_Label `fixes` added by @dhruvmanila on 2024-03-08 11:18_

---

_Label `help wanted` added by @MichaReiser on 2024-03-08 11:45_

---

_Closed by @charliermarsh on 2024-03-11 21:23_

---
