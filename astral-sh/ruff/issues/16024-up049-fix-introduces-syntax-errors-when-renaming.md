```yaml
number: 16024
title: "UP049 fix introduces syntax errors when renaming type parameters and behaves inconsistently in `typing.cast`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-02-07T15:57:31Z
updated_at: 2025-02-08T15:44:05Z
url: https://github.com/astral-sh/ruff/issues/16024
synced_at: 2026-01-10T11:09:57Z
```

# UP049 fix introduces syntax errors when renaming type parameters and behaves inconsistently in `typing.cast`

---

_Issue opened by @dscorbett on 2025-02-07 15:57_

### Description

The fix for [`private-type-parameter` (UP049)](https://docs.astral.sh/ruff/rules/private-type-parameter/) has some problems in 0.9.5.

Renaming a type parameter produces a syntax error when the character after the leading underscores is a digit.
```console
$ cat <<'# EOF' | ruff --isolated check --preview --select UP049 - --diff 2>&1 | grep error:
class C[_0]: ...
# EOF
error: Fix introduced a syntax error. Reverting all changes.
```

Renaming multiple type parameters in the same scope produces a syntax error when they have the same name, ignoring the leading underscores.
```console
$ cat >up049.py <<'# EOF'
class C[_T, __T]: ...
# EOF

$ ruff --isolated check --preview --select UP049 up049.py --fix
Found 2 errors (2 fixed, 0 remaining).

$ python up049.py
  File "up049.py", line 1
    class C[T, T]: ...
               ^
SyntaxError: duplicate type parameter 'T'
```

When a type parameter appears in `typing.cast` as a string with an escape sequence, the fixed version does not use a string. This doesn’t make any difference at runtime, but it is inconsistent with what happens when the string does not contain an escape sequence.
```console
$ cat <<'# EOF' | ruff --isolated check --preview --select UP049 - --fix
from typing import cast
def f[_T](): cast("_T", 1)
def f[_T](): cast("\u005fT", 1)
# EOF
from typing import cast
def f[T](): cast("T", 1)
def f[T](): cast(T, 1)
Found 2 errors (2 fixed, 0 remaining).
```

---

_Label `bug` added by @MichaReiser on 2025-02-07 16:24_

---

_Label `fixes` added by @MichaReiser on 2025-02-07 16:24_

---

_Comment by @AlexWaygood on 2025-02-07 16:50_

cc. @ntBre -- looks like we missed a few things here.

RUF052 also has to check whether the proposed new name is a valid identifier before passing it to the `Renamer`:

https://github.com/astral-sh/ruff/blob/46fe17767d3d5cf7b1f7729b024fca7821d19264/crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs#L233-L234

We could _possibly_ consider moving the logic to the `Renamer` itself since it seems like a _bit_ of a footgun, but it's unnecessary for some other uses of the `Renamer` such as the autofix for PYI025

---

_Comment by @InSyncWithFoo on 2025-02-07 18:21_

@ntBre @AlexWaygood I can take this one if you two don't mind.

---

_Comment by @AlexWaygood on 2025-02-07 18:22_

Fine by me, as long as @ntBre hasn't already started on it!

---

_Comment by @ntBre on 2025-02-07 18:22_

Sounds good! Thanks for checking!

---

_Assigned to @InSyncWithFoo by @ntBre on 2025-02-07 18:23_

---

_Comment by @AlexWaygood on 2025-02-08 11:28_

Following #16032, all problems have been fixed except this one:

> $ cat >up049.py <<'# EOF'
> class C[_T, __T]: ...
> # EOF
> 
> $ ruff --isolated check --preview --select UP049 up049.py --fix
> Found 2 errors (2 fixed, 0 remaining).
> 
> $ python up049.py
>   File "up049.py", line 1
>     class C[T, T]: ...
>                ^
> SyntaxError: duplicate type parameter 'T'

This is because we're applying both fixes as part of the same "apply fixes" loop in Ruff. I think I know how to fix this.

---

_Closed by @AlexWaygood on 2025-02-08 15:44_

---

_Closed by @AlexWaygood on 2025-02-08 15:44_

---
