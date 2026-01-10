---
number: 10278
title: E203 false positives for slices in format strings
type: issue
state: closed
author: sciyoshi
labels:
  - bug
assignees: []
created_at: 2024-03-07T18:23:23Z
updated_at: 2024-03-07T22:09:35Z
url: https://github.com/astral-sh/ruff/issues/10278
synced_at: 2026-01-10T01:22:49Z
---

# E203 false positives for slices in format strings

---

_Issue opened by @sciyoshi on 2024-03-07 18:23_

Sample code:

```python
a = f"{1[1 - 2 :]}"
b = 1[1 - 2 :]
```

When running with `ruff --select E203 --preview`, the first line triggers E203:

```
test.py:1:15: E203 [*] Whitespace before ':'
  |
1 | a = f"{1[1 - 2 :]}"
  |               ^ E203
  |
  = help: Remove whitespace before ':'
```

The same slice on the second line which is outside of a format string does not trigger E203.

---

_Referenced in [astral-sh/ruff#10280](../../astral-sh/ruff/pulls/10280.md) on 2024-03-07 18:26_

---

_Label `bug` added by @charliermarsh on 2024-03-07 21:59_

---

_Closed by @charliermarsh on 2024-03-07 22:09_

---

_Comment by @charliermarsh on 2024-03-07 22:09_

Thanks for the PR as always.

---
