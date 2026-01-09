---
number: 12616
title: PIE790 fix can make non-docstrings into docstrings
type: issue
state: closed
author: dscorbett
labels:
  - rule
  - fixes
  - help wanted
assignees: []
created_at: 2024-08-01T19:25:18Z
updated_at: 2024-11-18T02:30:07Z
url: https://github.com/astral-sh/ruff/issues/12616
synced_at: 2026-01-07T13:12:15-06:00
---

# PIE790 fix can make non-docstrings into docstrings

---

_Issue opened by @dscorbett on 2024-08-01 19:25_

There is one case where the fix for [PIE790](https://docs.astral.sh/ruff/rules/unnecessary-placeholder/) is not completely safe: it changes behavior when the placeholder statement is blocking a string from being a docstring.

```zsh
$ ruff --version
ruff 0.5.5
$ cat pie790.py
pass
"docstring?"
print(f"{__doc__=}")
$ python pie790.py
__doc__=None
$ ruff check --isolated --fix --select PIE790 pie790.py
Found 1 error (1 fixed, 0 remaining).
$ python pie790.py
__doc__='docstring?'
```

---

_Comment by @MichaReiser on 2024-08-01 21:23_

Interesting. It shouldn't be to hard to not flag the rule in this case. Just wondering, what's the real world code where you found this issue because a string literal probably indicates a missing assignment (or return, ...).?

---

_Label `rule` added by @MichaReiser on 2024-08-01 21:23_

---

_Label `help wanted` added by @MichaReiser on 2024-08-01 21:23_

---

_Comment by @dscorbett on 2024-08-01 22:04_

This is just something theoretical I noticed while reading the rule description. I agree that anything that looks like my sample code probably has a bug. I think PIE790 should still flag it, just not automatically fix it.

---

_Label `fixes` added by @AlexWaygood on 2024-08-02 11:11_

---

_Referenced in [astral-sh/ruff#14393](../../astral-sh/ruff/pulls/14393.md) on 2024-11-17 00:31_

---

_Closed by @charliermarsh on 2024-11-18 02:30_

---
