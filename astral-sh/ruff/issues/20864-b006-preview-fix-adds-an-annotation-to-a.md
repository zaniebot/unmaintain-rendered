---
number: 20864
title: B006 preview fix adds an annotation to a previously annotated variable
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2025-10-14T13:24:21Z
updated_at: 2025-10-15T01:14:02Z
url: https://github.com/astral-sh/ruff/issues/20864
synced_at: 2026-01-10T01:23:01Z
---

# B006 preview fix adds an annotation to a previously annotated variable

---

_Issue opened by @dscorbett on 2025-10-14 13:24_

### Summary

In preview, the fix for [`mutable-argument-default` (B006)](https://docs.astral.sh/ruff/rules/mutable-argument-default/) copies the parameter’s annotation into the body of the function, causing errors in some type-checkers. Even in type-checkers which allow reannotation, it is unnecessary. [Example](https://play.ruff.rs/40ba08a1-4799-46c2-a218-4f883b9862ed):
```console
$ cat <<'# EOF' | tee >b006.py b006_preview.py
def f(x: list[int] = []) -> int:
    return sum(x)
# EOF

$ ruff --isolated check b006_preview.py --select B006,RUF013 --unsafe-fixes --fix --preview
Found 2 errors (2 fixed, 0 remaining).

$ cat b006_preview.py
def f(x: list[int] | None = None) -> int:
    if x is None:
        x: list[int] = []
    return sum(x)

$ mypy b006_preview.py
b006_preview.py:3: error: Name "x" already defined on line 1  [no-redef]
b006_preview.py:4: error: Argument 1 to "sum" has incompatible type "list[int] | None"; expected "Iterable[bool]"  [arg-type]
Found 2 errors in 1 file (checked 1 source file)

$ pyright b006_preview.py | sed 's:/.*\(b006_preview\.py\):\1:'
b006_preview.py
  b006_preview.py:1:7 - error: Parameter declaration "x" is obscured by a declaration of the same name (reportRedeclaration)
1 error, 0 warnings, 0 informations
```

Compare the non-preview behavior:
```console
$ ruff --isolated check b006.py --select B006,RUF013 --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat b006.py
def f(x: list[int] | None = None) -> int:
    if x is None:
        x = []
    return sum(x)

$ mypy b006.py
Success: no issues found in 1 source file

$ pyright b006.py
0 errors, 0 warnings, 0 informations
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Comment by @dylwil3 on 2025-10-14 19:44_

Whoops that's my fault- I suggested that we do that to indicate type narrowing. I can take this, sorry!

---

_Label `bug` added by @dylwil3 on 2025-10-14 19:44_

---

_Label `fixes` added by @dylwil3 on 2025-10-14 19:44_

---

_Label `preview` added by @dylwil3 on 2025-10-14 19:44_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-10-14 19:51_

---

_Referenced in [astral-sh/ruff#20877](../../astral-sh/ruff/pulls/20877.md) on 2025-10-15 01:09_

---

_Closed by @dylwil3 on 2025-10-15 01:14_

---
