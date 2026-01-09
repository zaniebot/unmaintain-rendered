---
number: 21107
title: FURB122 fix introduces syntax error for rebound comprehension variable
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-28T13:28:22Z
updated_at: 2025-10-29T13:37:22Z
url: https://github.com/astral-sh/ruff/issues/21107
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB122 fix introduces syntax error for rebound comprehension variable

---

_Issue opened by @dscorbett on 2025-10-28 13:28_

### Summary

The fix for [`for-loop-writes` (FURB122)](https://docs.astral.sh/ruff/rules/for-loop-writes/) introduces a syntax error when the argument to `write` contains an assignment expression whose target is one of the targets of the `for` loop. The fix should be suppressed in that case. [Example](https://play.ruff.rs/4661b992-7660-4af1-88ca-fbf8b85e13fa):
```console
$ cat >furb122_1.py <<'# EOF'
with (
    open("furb122_1.py", encoding="utf-8") as src,
    open("furb122_1.txt", "w", encoding="utf-8") as dst,
):
    for line in src:
        dst.write(line := line.upper())
# EOF

$ python furb122_1.py; echo $?
0

$ ruff check --isolated furb122_1.py --select FURB122 --fix
invalid-syntax: assignment expression cannot rebind comprehension variable
 --> furb122_1.py:5:20
  |
3 |     open("furb122_1.txt", "w", encoding="utf-8") as dst,
4 | ):
5 |     dst.writelines(line := line.upper() for line in src)
  |                    ^^^^
  |

Found 2 errors (1 fixed, 1 remaining).
```

Ruff doesn’t detect the syntax error when the loop has multiple targets, but it’s still an error. [Example](https://play.ruff.rs/86fc2d19-cc14-4843-8f64-8014cf4cbefc):
```console
$ cat >furb122_2.py <<'# EOF'
with (
    open("furb122_2.py", encoding="utf-8") as src,
    open("furb122_2.txt", "w", encoding="utf-8") as dst,
):
    for first, *rest in src:
        dst.write(rest := "".join(rest))
# EOF

$ python furb122_2.py; echo $?
0

$ ruff check --isolated furb122_2.py --select FURB122 --fix
Found 1 error (1 fixed, 0 remaining).

$ python furb122_2.py 2>&1 | tail -n 3
    dst.writelines(rest := "".join(rest) for first, *rest in src)
                   ^^^^
SyntaxError: assignment expression cannot rebind comprehension iteration variable 'rest'
```

Since Python 3.12, it’s valid to rebind a variable an attribute of which is a loop target, in which case FURB122’s fix shouldn’t be suppressed. This issue’s fix will depend on the target version. [Example](https://play.ruff.rs/587e406a-f0b1-40b3-b4bd-24a1624cf716):
```console
$ cat >furb122_3.py <<'# EOF'
from types import SimpleNamespace
ns = SimpleNamespace(prev=[], last=None)
with (
    open("furb122_3.py", encoding="utf-8") as src,
    open("furb122_3.txt", "w", encoding="utf-8") as dst,
):
    for ns.last in src:
        dst.write((ns := SimpleNamespace(prev=[*ns.prev, ns.last], last=None)).prev[-1])
# EOF

$ python3.11 furb122_3.py; echo $?
0

$ ruff check --isolated furb122_3.py --select FURB122 --fix
Found 1 error (1 fixed, 0 remaining).

$ python3.11 furb122_3.py 2>&1 | tail -n 3
    dst.writelines((ns := SimpleNamespace(prev=[*ns.prev, ns.last], last=None)).prev[-1] for ns.last in src)
                    ^^
SyntaxError: assignment expression cannot rebind comprehension iteration variable 'ns'

$ python3.12 furb122_3.py; echo $?
0
```

### Version

ruff 0.14.2 (83a3bc4ee 2025-10-23)

---

_Label `bug` added by @ntBre on 2025-10-28 13:40_

---

_Label `fixes` added by @ntBre on 2025-10-28 13:40_

---

_Comment by @TaKO8Ki on 2025-10-28 22:47_

I will work on this issue.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-10-29 13:37_

---
