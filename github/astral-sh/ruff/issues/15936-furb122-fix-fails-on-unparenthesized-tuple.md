---
number: 15936
title: FURB122 fix fails on unparenthesized tuple
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-02-04T14:06:06Z
updated_at: 2025-02-05T09:16:56Z
url: https://github.com/astral-sh/ruff/issues/15936
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB122 fix fails on unparenthesized tuple

---

_Issue opened by @dscorbett on 2025-02-04 14:06_

### Description

Fixing [`for-loop-writes` (FURB122)](https://docs.astral.sh/ruff/rules/for-loop-writes/) fails in Ruff 0.9.4 when the iterable expression is an unparenthesized tuple.

If the argument to `write` is the loop target, the modified code raises an error at runtime.
```console
$ cat >furb122_1.py <<'# EOF'
with open("furb122.txt", "w") as f:
    for char in "a", "b":
        f.write(char)
# EOF

$ ruff --isolated check --preview --select FURB122 furb122_1.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb122_1.py
with open("furb122.txt", "w") as f:
    f.writelines("a", "b")

$ python furb122_1.py
Traceback (most recent call last):
  File "furb122_1.py", line 2, in <module>
    f.writelines("a", "b")
TypeError: _IOBase.writelines() takes exactly one argument (2 given)
```

Otherwise, the fix introduces a syntax error.
```console
$ cat >furb122_2.py <<'# EOF'
with open("furb122.txt", "w") as f:
    for char in "a", "b":
        f.write(f"{char}")
# EOF

$ ruff --isolated check --preview --select FURB122 furb122_2.py --unsafe-fixes --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `furb122_2.py`, the rule codes FURB122, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

---

_Label `bug` added by @AlexWaygood on 2025-02-04 15:58_

---

_Label `fixes` added by @AlexWaygood on 2025-02-04 15:58_

---

_Label `help wanted` added by @AlexWaygood on 2025-02-04 15:58_

---

_Comment by @InSyncWithFoo on 2025-02-04 20:46_

[`FURB142`](https://docs.astral.sh/ruff/rules/for-loop-set-mutations/) has the same problem:

```shell
$ ruff check --isolated --fix --fix-only --select FURB142 --preview -
s = set()
for a in 1, 2:
    s.add(f'{a}')

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `-`, the rule codes FURB142, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

In fact, it also suffers from #15725.

---

_Referenced in [astral-sh/ruff#15953](../../astral-sh/ruff/pulls/15953.md) on 2025-02-05 00:07_

---

_Closed by @MichaReiser on 2025-02-05 09:16_

---
