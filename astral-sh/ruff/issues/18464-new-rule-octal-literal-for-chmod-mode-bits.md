---
number: 18464
title: "[New rule] Octal literal for `chmod` mode bits"
type: issue
state: closed
author: opk12
labels:
  - rule
assignees: []
created_at: 2025-06-04T16:11:26Z
updated_at: 2025-06-19T09:30:30Z
url: https://github.com/astral-sh/ruff/issues/18464
synced_at: 2026-01-10T01:22:59Z
---

# [New rule] Octal literal for `chmod` mode bits

---

_Issue opened by @opk12 on 2025-06-04 16:11_

### Summary

`pathlib.Path('/file').chmod(444)` means something different than what it seems, should be `chmod(0o444)`.

Also, `chmod` of a decimal or hexadecimal literal is most likely unintentional, because it is are hard to read and the standard usage is octal. For example, telling the mode bits from `292` or `0x124` is not straightforward.

A new rule could look for a numeric literal and propose the octal form. There exist `stat.S_*` constants listed at [os.chmod()](https://docs.python.org/3/library/os.html#os.chmod), but honestly I'm not their fan, as the 3-digit version is cleaner and is standard in the POSIX world.

I don't have experience with the similar functions, but I guess the rule should also cover them.
* `pathlib.Path.lchmod()`
* `os.chmod()` 
* `os.fchmod()`
* `os.lchmod()`

Docs
* [Path.chmod()](https://docs.python.org/3/library/pathlib.html#pathlib.Path.chmod)
* [os.chmod()](https://docs.python.org/3/library/os.html#os.chmod)

---

_Label `rule` added by @ntBre on 2025-06-04 17:43_

---

_Comment by @ntBre on 2025-06-04 17:50_

This makes sense to me. I think we should probably also allow the `stat.* | ...` case, but I think it makes sense to warn on other int literals.

---

_Comment by @MichaReiser on 2025-06-05 06:26_

https://docs.astral.sh/ruff/rules/bad-file-permissions/ is related. We should ensure the two rules perform the check in the same places

---

_Comment by @RazerM on 2025-06-05 23:33_

I have a proof of concept of this working and hope to open a PR soon™

```
crates\ruff_linter\resources\test\fixtures\ruff\RUF061.py:3:18: RUF061 Non-octal mode, did you mean 0o444?
  |
1 | import os
2 |
3 | os.chmod("/foo", 444)  # Error
  |                  ^^^ RUF061
4 | os.chmod("/foo", 0o444)  # OK
  |
```

---

_Comment by @ntBre on 2025-06-05 23:37_

Awesome, thank you! Just a note if it's not too late already, I think RUF061-063 are all in use by existing PRs (https://github.com/astral-sh/ruff/pull/18233#discussion_r2127241647). We can always  renumber later, but RUF064 should be the next free code.

---

_Assigned to @RazerM by @ntBre on 2025-06-05 23:38_

---

_Referenced in [astral-sh/ruff#18541](../../astral-sh/ruff/pulls/18541.md) on 2025-06-07 22:26_

---

_Closed by @MichaReiser on 2025-06-19 09:30_

---
